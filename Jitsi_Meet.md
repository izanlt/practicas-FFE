
# DOCUMENTACIÓN PROYECTO JITSI MEET

## Creación de máquinas virtuales

Antes de todo, debemos realizar la creación de las máquinas virtuales, que deben ser las siguientes:
- Una máquina de Ubuntu Server en la versión 22.04.
- Otra máquina de Ubuntu Desktop.
- Por último, una máquina de Windows, que podemos elegir entre Windows 10 y Windows 11.

---

## Configuración de SSH en Ubuntu Server

Dentro de la máquina virtual de Ubuntu Server, instalamos SSH con el siguiente comando:

```
sudo apt update
sudo apt install openssh
```

---

## Configuración de IP estática

### En Ubuntu Server

Editamos el archivo de configuración de red para obtener internet en la máquina:

```
sudo nano /etc/netplan/00-installer-config.yaml
```
También se puede editar el archivo de configuración de red que existe dentro de nuestra máquina, que es el siguiente:

```
sudo nano /etc/netplan/50-cloud-init.yaml
```

Agregamos los datos necesarios para la IP estática (ejemplo):

```
network:
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
    eth1:
      dhcp4: yes
version: 2
```

Guardamos los cambios:

```
sudo netplan apply
```

---

### En Ubuntu Desktop

Ejecutamos el siguiente comando y editamos el fichero:

```
sudo nano /etc/netplan/01-network-manager-all.yaml
```

En el fichero escribimos la configuración de IP estática correspondiente. 

```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.101/24]
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Para guardar los cambios de red introduciremos el siguiente comando:

```
sudo netplan apply
```

---

### En Windows

1. Abrimos el panel de control (`Windows + R` > escribir `control`).
2. Vamos a **Redes e Internet** > **Centro de redes y recursos compartidos**.
3. Seleccionamos **Cambiar configuración del adaptador**.
4. Entramos en las propiedades del adaptador de red.
5. Editamos el **Protocolo de Internet versión 4**.
6. Introducimos la IP estática correspondiente.

---

## Instalación del servidor Jitsi Meet

Primero debemmos actualizar el sistema:

```
sudo apt update && sudo apt upgrade
```

Verificar compatibilidad con repositorios HTTPS:

```
sudo apt install apt-transport-https
```

Añadimos el repositorio `universe`:

```
sudo add-apt-repository universe
```

Y actualizamos nuevamente:

```
sudo apt update
```

---

### Adición de repositorios:

Repositorio de **Prosody:**

```
sudo curl -sL https://prosody.im/files/prosody-debian-packages.key -o /etc/apt/keyrings/prosody-debian-packages.key
echo "deb [signed-by=/etc/apt/keyrings/prosody-debian-packages.key] http://packages.prosody.im/debian $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/prosody-debian-packages.list
sudo apt install lua5.2
```

Repositorio para **Jitsi:**

```
curl -sL https://download.jitsi.org/jitsi-key.gpg.key | sudo sh -c 'gpg --dearmor > /usr/share/keyrings/jitsi-keyring.gpg'
echo "deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/" | sudo tee /etc/apt/sources.list.d/jitsi-stable.list
```

### Apertura de los puertos en el firewall:

```
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 10000/udp
sudo ufw allow 22/tcp
sudo ufw allow 3478/udp
sudo ufw allow 5349/tcp
sudo ufw enable
```

Si queremos comprobar el estado del firewall, podemos ejecutar el siguiente comando:

```
sudo ufw status verbose
```
---

### Instalar Jitsi Meet

Para la instalación del servidor de Jitsi Meet debemos ejecutar el siguiente comando:

```
sudo apt install jitsi-meet
```

Durante la instalación:

- Introducir el dominio (por ejemplo: `jitsimeetizan.duckdns.org`).
- Introducir correo electrónico.
- También en algún momento nos pedirá el número de teléfono, pero **NO** es necesario.
- En la parte del certificado, seleccionar la tercera opción (generar un certificado autofirmado).

---

## Configurar direcciones IP en Jitsi

Editar el archivo de configuración `jvb.conf`:

Unificaremos el contenido del archivo ya que hay dos bloques ice4j:

```
ice4j {
  harvest {
    mapping {
      aws {
        enabled = false
      }
      stun {
        addresses = ["meet-jit-si-turnrelay.jitsi.net:443"]
      }
      static-mappings = [
        {
          local-address = "192.168.1.200"
          public-address = "80.58.95.131"
        }
      ]
    }
  }
}

```

---

## Configurar Jitsi para más de 100 participantes

Editar el archivo `/etc/systemd/system.conf`:

```
sudo nano /etc/systemd/system.conf
```

Cambiar los valores necesarios, como:

![Captura valores 1](/imagenes/valores.jpg) ![Captura valores 1](/imagenes/valores2.jpg)

---

## Configurar certificado SSL

### Ir al directorio de certificados:

```
cd /etc/ssl
```

### Crear certificado:

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/jitsi.key -out /etc/ssl/certs/jitsi.crt -subj "/CN=jitsimeetizan.duckdns.org"
```

### Editar la configuración de nginx (utilizo mi dominio como ejemplo):

```
sudo nano /etc/nginx/sites-available/jitsimeetizan.duckdns.org.conf
```

### Reemplazar estas líneas:

```
ssl_certificate /etc/ssl/certs/jitsi.crt;
ssl_certificate_key /etc/ssl/private/jitsi.key;
```

---

## Reinicio de servicios

Reiniciamos los servicios de Jitsi Meet mediante los siguientes comandos:

```
sudo systemctl reload nginx
sudo systemctl restart prosody
sudo systemctl restart jicofo
sudo systemctl restart jitsi-videobridge2
```
---

# AUTENTICACIÓN

Entramos en el siguiente fichero por medio de nuestro **hostname**, en el que editaremos lo necesario para la autenticación:

```
sudo nano /etc/prosody/conf.avail/jitsimeetizan.duckdns.org.cfg.lua
```

Dentro del fichero, editaremos lo siguiente:

- Habilitación de la autenticación

Cambiamos en el bloque de `VirtualHost "<hostname>"`, en la parte de `authentication` que viene por defecto con el valor de `anonymous`. Lo cambiaremos a `"internal_hashed"` para activar la autenticación:

```
VirtualHost "jitsimeetizan.duckdns.org"
    authentication = "internal_hashed"
```

Añadimos el siguiente bloque al fichero que estamos editando para que el inicio de sesión sea **ANÓNIMO**:

```
VirtualHost "guest.jitsimeetizan.duckdns.org"
    authentication = "anonymous"
    c2s_require_encryption = false
```

---

## Configuración de Jitsi Meet

Entramos en el siguiente fichero:

```
sudo nano /etc/jitsi/meet/jitsimeetizan.duckdns.org-config.js
```

Añadimos la parte de `anonymousdomain` en el fichero, en el bloque de `var config`:

```
var config = {
    // Connection
    hosts: {
        domain: 'jitsimeetizan.duckdns.org',
        anonymousdomain: 'guest.jitsimeetizan.duckdns.org',
    }
}
```

---

## Configuración de Jicofo

Entramos en el archivo de Jicofo para configurarlo:

```
sudo nano /etc/jitsi/jicofo/jicofo.conf
```

Agregamos las siguientes líneas, con el tipo XMPP:

```
authentication: {
    enabled: true
    type: XMPP
    login-url: jitsimeetizan.duckdns.org
}
```

---

## Creación de administradores

Primero crearemos un usuario normal con el siguiente comando:

```
sudo prosodyctl adduser admin@jitsimeetizan.duckdns.org
```

Al introducir el último comando, la salida es la siguiente. El rol que tiene al crearlo es de miembro, por lo que debemos cambiarlo a administrador.

```
OK: Created admin@jitsimeetizan.duckdns.org with role 'prosody:member'
```

Para convertirlo en administrador, editamos:

```
sudo nano /etc/prosody/conf.avail/jitsimeetizan.duckdns.org.cfg.lua
```

Y agregamos este bloque:

```
admins = { "admin@jitsimeetizan.duckdns.org" }
```

En este bloque agregaremos los administradores que deseemos.

---

## Reinicio de servicios

Reiniciamos los servidores de Jitsi:

```
sudo systemctl restart prosody
sudo systemctl restart jicofo
sudo systemctl restart jitsi-videobridge2
```
---


# Personalización y Automatización en Jitsi Meet

## Personalización de la interfaz

Para realizar los cambios que nos indica la práctica, tendremos que editar el siguiente archivo:

```
sudo nano /usr/share/jitsi-meet/interface_config.js
```

## Cambio de título

Dentro de ese archivo, buscamos la siguiente línea y la modificamos para establecer el título personalizado:

```
APP_NAME: 'Mi Jitsi personalizado',
```

Puedes cambiar `'Mi Jitsi personalizado'` por el texto que desees mostrar como título en la interfaz del sitio.

## Cambiar el título de la cabecera

Para cambiar el **título de la cabecera**, tenemos que editar el archivo:

```
/usr/share/jitsi-meet/lang/main-es.json
```

Buscar la clave `headerTitle` y cambiarla por el texto que se desee.

Ejemplo:

```
"headerTitle": "Bienvenido a mi Jitsi"
```

## Deshabilitar compartir pantalla y chat

Editamos el archivo de configuración:

```
sudo nano /etc/jitsi/meet/jitsimeetizan.duckdns.org-config.js
```

Dentro del bloque `var config`, agregamos:

```
var config = {
    disableChat: true,
    disableScreenSharing: true,
    // ...
};
```

## Mensaje de bienevnida

Ya que en el archivo `interface_config.js` no se me actualizaba el mensaje de bienvenida en el servidor, lo cambié en otro archivo:

```
sudo nano /usr/share/jitsi-meet/static/welcomePageAdditionalContent.html
```

En el archivo hay que introducirle el texto en lenguaje **HTML**, en mi caso le escribí lo siguiente:

```
<template id = "welcome-page-additional-content-template"></template>
<div style="text-align:center;">
    <h2>¡Bienvenido a Mi Jitsi!</h2>
    <p>Por favor, espera a que el moderador inicie la reunión.</p>
</div>

```

## Introducción de una imagen de Windows al SSH

Para ejecutar este comando, primero debemos cerrar la sesión de nuestro servidor en el SSH.
Después tendremos que introducir el siguiente comando:

```
scp "D:\\Users\\Alumno_m\\Downloads\\imagenes\\tu-imagen" usuario@tu-dominio:/usr/share/jitsi-meet/images/
```

Esto sirve para introducir una imagen de Windows a la carpeta de imágenes del servidor de Jitsi Meet, a través de la cuál se podrán editar los logos del mismo.
En mi caso, la imagen estaba en una carpeta llamada imágenes dentro del directorio de Descargas.

## Cambiar los logos del servidor Jitsi Meet

Primero nos introducimos al archivo `interface_config.js`:

```
sudo nano /usr/share/jitsi-meet/interface_config.js
```

Posteriormente editaremos las siguientes opciones en las que introduciremos la ruta de la imagen de nuestro logo (puede ser en **PNG**)

```
DEFAULT_WELCOME_PAGE_LOGO_URL: 'images/tu-logo.png',
DEFAULT_LOGO_URL: 'images/tu-logo.png',
```
La segunda línea que se debe cambiar, está comentada con //. Las // hay que suprimirlas e introducir nuestro logo

---

## Automatización y administración

### Crear script para crear usuarios

Creamos el siguiente script:

```
sudo nano crear_usuario.sh
```

Dentro del archivo, agregamos:

```
echo "Introduce el nombre del nuevo usuario:"
read username
password=$(openssl rand -base64 12)
useradd -m -s /bin/bash "$username"
echo "$username:$password" | chpasswd
echo "Usuario '$username' creado con contraseña: $password"
```

---

### Crear script para backup

A continuación crearemos un script de respaldo:

```
sudo nano backup_jitsi.sh
```

El contenido del script será el siguiente:

```
config_dir="/etc/jitsi"
backup_dir="/backup/jitsi"
fecha=$(date '+%Y-%m-%d_%H-%M-%S')
mkdir -p "$backup_dir"
cp -r "$config_dir" "$backup_dir/jitsi_backup_$fecha"
echo "Copia de seguridad guardada en $backup_dir/jitsi_backup_$fecha"
```

# Video de funcionamiento del servidor

https://youtu.be/WpKnAyUT_ZY
