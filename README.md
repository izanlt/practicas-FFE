
# DOCUMENTACIÓN PROYECTO JITSI MEET

## 1. Creación de Máquinas Virtuales

Creamos las máquinas virtuales indicadas en la práctica usando **Hyper-V**.

---

## 2. Configuración de SSH en Ubuntu Server

Dentro de la máquina virtual de Ubuntu Server, instalamos SSH ejecutando:

```
sudo apt update
sudo apt install openssh
```

---

## 3. Configuración de IP Estática

### En Ubuntu Server

Editamos el archivo de configuración de red:

```
sudo nano /etc/netplan/00-installer-config.yaml
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

Y escribimos la configuración de IP estática correspondiente. Luego aplicamos:

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

## 4. Instalación del Servidor Jitsi Meet

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
sudo curl https://packages.prosody.im/debian/prosody-archive.key -o /usr/share/keyrings/prosody-archive.key
echo 'deb [signed-by=/usr/share/keyrings/prosody-archive.key] https://packages.prosody.im/debian bookworm main' | sudo tee /etc/apt/sources.list.d/prosody.list
sudo apt update
```

**Para Jitsi:**

```
curl https://download.jitsi.org/jitsi-key.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jitsi-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/' | sudo tee /etc/apt/sources.list.d/jitsi-stable.list
sudo apt update
```

---

### Abrir puertos en el firewall:

```
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 10000/udp
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

Editar el archivo de configuración:

```
sudo nano /etc/jitsi/videobridge/sip-communicator.properties
```

Agregar al final:

```
org.ice4j.ice.harvest.NAT_HARVESTER_LOCAL_ADDRESS=192.168.1.200
org.ice4j.ice.harvest.NAT_HARVESTER_PUBLIC_ADDRESS=80.58.95.131
```

O unificar en formato estructurado (en otro archivo como `jvb.conf`):

```
ice4j {
  harvest {
    mapping {
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

## 6. Configurar Jitsi para 100 participantes

Editar el archivo:

```
sudo nano /etc/jitsi/meet/tu-dominio-config.js
```

Cambiar los valores necesarios, como:


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
