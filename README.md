
# DOCUMENTACIÓN PROYECTO JITSI MEET

## 1. Creación de máquinas virtuales

Antes de todo, debemos realizar la creación de las máquinas virtuales, que deben ser las siguientes:
- Una máquina de Ubuntu Server en la versión 22.04.
- Otra máquina de Ubuntu Desktop.
- Por último, una máquina de Windows, que podemos elegir entre Windows 10 y Windows 11.

---

## 2. Configuración de SSH en Ubuntu Server

Dentro de la máquina virtual de Ubuntu Server, instalamos SSH ejecutando:

```
sudo apt update
sudo apt install openssh
```

---

## 3. Configuración de IP estática

### En Ubuntu Server

Editamos el archivo de configuración de red:

```
sudo nano /etc/netplan/00-installer-config.yaml
```
También se puede editar el archivo de configuración de red que existe en el directorio, que es el siguiente:
```
sudo nano /etc/netplan/50-cloud-init.yaml
```

Agregamos los datos necesarios para la IP estática (ejemplo):

```yaml
network:
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
    eth1:
      dhcp4: yes
version: 2
```

Guardamos y aplicamos los cambios:

```
sudo netplan apply
```

---

### En Ubuntu Desktop

Ejecutamos:

```
sudo nano /etc/netplan/01-network-manager-all.yaml
```

Y escribimos la configuración de IP estática correspondiente. 

```yaml
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

Para guardar los cambios de red aplicaremos el siguiente comando:

```
sudo netplan apply
```

---

### En Windows

1. Abrimos el panel de control (`Windows + R` > escribir `control`).
2. Vamos a **Redes e Internet** > **Centro de redes y recursos compartidos**.
3. Seleccionamos **Cambiar configuración del adaptador**.
4. Entramos en las propiedades del adaptador de red.
5. Editamos el **Protocolo de Internet versión 4 (TCP/IPv4)**.
6. Introducimos la IP estática correspondiente.

---

## 4. Instalación del servidor Jitsi Meet

### Actualizar el sistema:

```
sudo apt update && sudo apt upgrade
```

### Verificar compatibilidad con repositorios HTTPS:

```
sudo apt install apt-transport-https
```

### Habilitar el repositorio `universe`:

```
sudo add-apt-repository universe
```

Y actualizamos nuevamente:

```
sudo apt update
```

---

### Añadir repositorios:

**Prosody:**

```
sudo curl -sL https://prosody.im/files/prosody-debian-packages.key -o /etc/apt/keyrings/prosody-debian-packages.key
echo "deb [signed-by=/etc/apt/keyrings/prosody-debian-packages.key] http://packages.prosody.im/debian $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/prosody-debian-packages.list
sudo apt install lua5.2
```

**Para Jitsi:**

```
curl -sL https://download.jitsi.org/jitsi-key.gpg.key | sudo sh -c 'gpg --dearmor > /usr/share/keyrings/jitsi-keyring.gpg'
echo "deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/" | sudo tee /etc/apt/sources.list.d/jitsi-stable.list
```

Y actualizaremos las fuentes de los paquetes:

```
sudo apt install
```
---

### Abrir puertos en el firewall:

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

```
sudo apt install jitsi-meet
```

Durante la instalación:

- Introducir el dominio (por ejemplo: `jitsimeetizan.duckdns.org`).
- Introducir correo electrónico.
- También en algún momento nos pedirá el número de teléfono, pero **NO** es necesario.
- En la parte del certificado, seleccionar la tercera opción (generar auto-firmado).

---

## 5. Configurar direcciones IP en Jitsi

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

## 6. Configurar Jitsi para más de 100 participantes

Editar el archivo `/etc/systemd/system.conf`:

```
sudo nano /etc/systemd/system.conf
```

Cambiar los valores necesarios, como:

![Captura valores 1](/imagenes/valores.jpg) ![Captura valores 1](/imagenes/valores2.jpg)

---

## 7. Configurar certificado SSL

Ir al directorio de certificados:

```
cd /etc/ssl
```

Crear certificado autofirmado:

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/jitsi.key -out /etc/ssl/certs/jitsi.crt -subj "/CN=jitsimeetizan.duckdns.org"
```

Editar la configuración de nginx:

```
sudo nano /etc/nginx/sites-available/tu-dominio.conf
```

Reemplazar estas líneas:

```
ssl_certificate /etc/ssl/certs/jitsi.crt;
ssl_certificate_key /etc/ssl/private/jitsi.key;
```

---

## 8. Reiniciar los servicios

```
sudo systemctl reload nginx
sudo systemctl restart prosody
sudo systemctl restart jicofo
sudo systemctl restart jitsi-videobridge2
```

# AUTENTICACIÓN

Entramos en el siguiente fichero por medio de nuestro **hostname**, en el que editaremos lo necesario para la autenticación:

```bash
sudo nano /etc/prosody/conf.avail/jitsimeetizan.duckdns.org.cfg.lua
```

Dentro del fichero, editaremos lo siguiente:

- Habilitación de la autenticación

Cambiamos en el bloque de `VirtualHost "<hostname>"`, en la parte de `authentication` que viene por defecto con el valor de `anonymous`. Lo cambiaremos a `"internal_hashed"` para activar la autenticación:

```lua
VirtualHost "jitsimeetizan.duckdns.org"
    authentication = "internal_hashed"
```

Añadimos el siguiente bloque al fichero que estamos editando para que el **inicio de sesión sea anónimo**:

```lua
VirtualHost "guest.jitsimeetizan.duckdns.org"
    authentication = "anonymous"
    c2s_require_encryption = false
```

---

## Configuración de Jitsi Meet

Entramos en el siguiente fichero:

```bash
sudo nano /etc/jitsi/meet/jitsimeetizan.duckdns.org-config.js
```

Añadimos la parte de `anonymousdomain` en el fichero, en el bloque de `var config`:

```javascript
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

Entramos en este archivo y le agregamos la siguiente línea:

```bash
sudo nano /etc/jitsi/jicofo/jicofo.conf
```

Agregamos lo siguiente:

```hocon
authentication: {
    enabled: true
    type: XMPP
    login-url: jitsimeetizan.duckdns.org
}
```

---

## Creación de administradores

Primero crearemos un usuario normal con el siguiente comando:

```bash
sudo prosodyctl adduser admin@jitsimeetizan.duckdns.org
```

Salida esperada:

```text
OK: Created admin@jitsimeetizan.duckdns.org with role 'prosody:member'
```

Para convertirlo en administrador, editamos:

```bash
sudo nano /etc/prosody/conf.avail/jitsimeetizan.duckdns.org.cfg.lua
```

Y agregamos este bloque:

```lua
admins = { "admin@jitsimeetizan.duckdns.org" }
```

En este bloque agregaremos los administradores que deseemos.

---

## Reinicio de servicios

Reiniciamos los servidores de Jitsi:

```bash
sudo systemctl restart prosody
sudo systemctl restart jicofo
sudo systemctl restart jitsi-videobridge2
```

## Personalización de la interfaz

Para realizar los cambios que nos indica la práctica, tendremos que editar el siguiente archivo:

```bash
sudo nano /usr/share/jitsi-meet/interface_config.js
```

---

### Cambio de título

Dentro de ese archivo, buscamos la siguiente línea y la modificamos para establecer el título personalizado:

```javascript
APP_NAME: 'Mi Jitsi Personalizado',
```

Puedes cambiar `'Mi Jitsi Personalizado'` por el texto que desees mostrar como título en la interfaz del sitio.


# Personalización y Automatización en Jitsi Meet

## Cambiar el título de la cabecera

Para cambiar el **título de la cabecera**, tenemos que editar el archivo:

```
/usr/share/jitsi-meet/lang/main-es.json
```

Buscar la clave `headerTitle` y cambiarla por el texto deseado.

Ejemplo:

```json
"headerTitle": "¡Bienvenido a Mi Jitsi!"
```

## Deshabilitar compartir pantalla y chat

Editamos el archivo de configuración:

```bash
sudo nano /etc/jitsi/meet/jitsimeetizan.duckdns.org-config.js
```

Dentro del bloque `var config`, agregamos:

```javascript
var config = {
    disableChat: true,
    disableScreenSharing: true,
    // ...
};
```

---

## Automatización y administración

### Crear script para crear usuarios

Creamos el siguiente script:

```bash
sudo nano crear_usuario.sh
```

Dentro del archivo, agregamos:

```bash
#!/bin/bash

echo "Introduce el nombre del nuevo usuario:"
read username

# Generar contraseña aleatoria
password=$(openssl rand -base64 12)

# Crear usuario con la contraseña
useradd -m -s /bin/bash "$username"
echo "$username:$password" | chpasswd

# Mostrar información
echo "Usuario '$username' creado con contraseña: $password"
```

---

### Crear script para backup

Ahora creamos un script para respaldar la configuración de Jitsi:

```bash
sudo nano backup_jitsi.sh
```

Contenido del script:

```bash
#!/bin/bash

# Directorio de origen
config_dir="/etc/jitsi"

# Directorio de respaldo
backup_dir="/backup/jitsi"

# Fecha actual
fecha=$(date '+%Y-%m-%d_%H-%M-%S')

# Crear respaldo
mkdir -p "$backup_dir"
cp -r "$config_dir" "$backup_dir/jitsi_backup_$fecha"

echo "Copia de seguridad guardada en $backup_dir/jitsi_backup_$fecha"
```
