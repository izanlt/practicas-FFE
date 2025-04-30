
# DOCUMENTACIÓN PROYECTO JITSI MEET

## 1. Creación de Máquinas Virtuales

Creamos las máquinas virtuales indicadas en la práctica usando **Hyper-V**.

---

## 2. Configuración de SSH en Ubuntu Server

Dentro de la máquina virtual de Ubuntu Server, instalamos SSH ejecutando:

```bash
sudo apt update
sudo apt install openssh
```

---

## 3. Configuración de IP Estática

### En Ubuntu Server

Editamos el archivo de configuración de red:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Agregamos los datos necesarios para la IP estática (ejemplo):

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - 192.168.1.200/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

Guardamos y aplicamos los cambios:

```bash
sudo netplan apply
```

---

### En Ubuntu Desktop

Ejecutamos:

```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```

Y escribimos la configuración de IP estática correspondiente. Luego aplicamos:

```bash
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

```bash
sudo apt update && sudo apt upgrade
```

### Verificar compatibilidad con repositorios HTTPS:

```bash
sudo apt install apt-transport-https
```

### Habilitar el repositorio `universe`:

```bash
sudo add-apt-repository universe
```

Y actualizamos nuevamente:

```bash
sudo apt update
```

---

### Añadir repositorios:

```bash
sudo curl https://packages.prosody.im/debian/prosody-archive.key -o /usr/share/keyrings/prosody-archive.key
echo 'deb [signed-by=/usr/share/keyrings/prosody-archive.key] https://packages.prosody.im/debian bookworm main' | sudo tee /etc/apt/sources.list.d/prosody.list
sudo apt update
```

Para Jitsi:

```bash
curl https://download.jitsi.org/jitsi-key.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/jitsi-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/' | sudo tee /etc/apt/sources.list.d/jitsi-stable.list
sudo apt update
```

---

### Abrir puertos en el firewall (si aplica):

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 10000/udp
```

---

### Instalar Jitsi Meet

```bash
sudo apt install jitsi-meet
```

Durante la instalación:

- Introducir el dominio (por ejemplo: `jitsimeetizan.duckdns.org`).
- Introducir correo electrónico.
- En la parte del certificado, seleccionar la tercera opción (generar auto-firmado).

---

## 5. Configurar direcciones IP en Jitsi

Editar el archivo de configuración:

```bash
sudo nano /etc/jitsi/videobridge/sip-communicator.properties
```

Agregar al final:

```
org.ice4j.ice.harvest.NAT_HARVESTER_LOCAL_ADDRESS=192.168.1.200
org.ice4j.ice.harvest.NAT_HARVESTER_PUBLIC_ADDRESS=80.58.95.131
```

O unificar en formato estructurado (en otro archivo como `jvb.conf`):

```hocon
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

```bash
sudo nano /etc/jitsi/meet/tu-dominio-config.js
```

Cambiar los valores necesarios, como:

```javascript
channelLastN: 100,
```

Y otros parámetros relacionados si se requiere (p. ej., `startAudioOnly`, `startWithVideoMuted`, etc.).

---

## 7. Configurar certificado SSL

Ir al directorio de certificados:

```bash
cd /etc/ssl
```

Crear certificado autofirmado:

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/jitsi.key -out /etc/ssl/certs/jitsi.crt -subj "/CN=jitsimeetizan.duckdns.org"
```

Editar la configuración de nginx:

```bash
sudo nano /etc/nginx/sites-available/tu-dominio.conf
```

Reemplazar estas líneas:

```nginx
ssl_certificate /etc/ssl/certs/jitsi.crt;
ssl_certificate_key /etc/ssl/private/jitsi.key;
```

---

## 8. Reiniciar los servicios

```bash
sudo systemctl reload nginx
sudo systemctl restart prosody
sudo systemctl restart jicofo
sudo systemctl restart jitsi-videobridge2
```

---

## ✅ ¡Servidor Jitsi Meet instalado y configurado con éxito!
