# DOCUMENTACIÓN PROYECTO PROXMOX (TEDRA)

---

## Creación de máquinas virtuales

Para realizar la siguiente práctica, debemos crear dos máquinas virtuales con la iso de Proxmox y con una capacidad de 6144 MB de RAM.
Para encontrar la iso de Proxmox, realizamos una búsqueda en el buscador de Google, 
y en la página oficial de Proxmox encontramos en la parte superior derecha, el apartado de `Downloads`

![image](https://github.com/user-attachments/assets/9a44e973-3701-4df2-bed6-b31c6db1a606)

Hacemos clic en Downloads y descargamos el primer archivo, que es la iso de **Proxmox**.

![image](https://github.com/user-attachments/assets/ddb86622-596c-4810-9048-f374cd449c60)

Una vez instalada, nos dirigimos a Hyper-V para crear las máquinas virtuales.
Le ponemos la memoria RAM que nos indiica el enunciado de la práctica.

![image](https://github.com/user-attachments/assets/ed7274d0-fac3-4ec2-be5e-3c1b6c5b9356)

Y después le introducimos la iso que descargamos anteriormente.

![image](https://github.com/user-attachments/assets/04f6fb7c-9001-4bc2-9c29-7d69413a1e4f)

Una vez realizados estos pasos, la ejecutamos.

![image](https://github.com/user-attachments/assets/eee7cb9a-d636-435d-ae42-a22d5bc9d283)

## Instalación Proxmox

Después de ejecutarla, nos sale la siguiente pantalla, en la que elegiremos la instalación desde el entorno gráfico.

![image](https://github.com/user-attachments/assets/cc49404f-8ddb-4ffa-9fc0-9d8a0da46ba7)

Aceptamos los términos de licencia.

![image](https://github.com/user-attachments/assets/fbf4baa4-6489-4f5d-b628-ff00fb4a3adf)

Seleccioanamos el ext4.

![image](https://github.com/user-attachments/assets/4fe19bc1-3b0d-40d9-a0f5-b1376f02372f)

En cuanto al idioma, seleccionaremos España y la zona horaria de Madrid.

![image](https://github.com/user-attachments/assets/dfc2649d-99dd-481f-bbfd-e2eed6712b14)

Introducimos nuestra contraseña y nuestro correo eléctronico.

![image](https://github.com/user-attachments/assets/786c5f10-9c5e-49fa-97c6-ce82eae4d21d)

Introducimos las IPs y le damos a "Next".

![image](https://github.com/user-attachments/assets/b855190a-5df5-411c-9f35-c7e5dbca2772)

Procedemos con la instalación.

![image](https://github.com/user-attachments/assets/61e14be5-4986-4954-8a4f-3edb411f4198)

Cuando se ha instalado, entramos en la configuración de la máquina y eliminamos la iso de proxmox:

![image](https://github.com/user-attachments/assets/fb168ec7-7cd9-498b-bc62-a7a41deef349)

---

## Creación cluster

Después de realizar los pasos anteriores, debemos ejecutar la máquina virtual otra vez para que no se repita en bucle la instalación de Proxmox 
La volvemos a ejecutar y nos aparece el siguiente mensaje en la pantalla. 

![image](https://github.com/user-attachments/assets/4f5f089c-5997-4a2e-8ac6-4ebc0d34ee9f)

Lo que debemos hacer es entrar en nuestro buscador y escribir la dirección que nos aparece, en mi caso es la siguiente: 

```
https://172.23.42.121:8006/
```

De esta manera podremos acceder a nuestro servidor de Proxmox.

![image](https://github.com/user-attachments/assets/01d760eb-e1d9-41e4-9cf5-a230e08301e8)

Cuando entramos en el servidor tenemos que rellenar los siguientes campos, en el que el nombre de usuario será root, y la contraseña es la que le establecimos durante la instalación del servidor.

![image](https://github.com/user-attachments/assets/c0f6e77e-f672-41d3-8351-80ea518be94c)

Ahora crearemos la otra máquina con la iso de Proxmox, en la que le introduciremos la misma configuración, pero cambiandole el nombre de pve a pve2 y cambiandole la ip.

![image](https://github.com/user-attachments/assets/f53b842a-ed7d-4ab8-bf9b-734cb9fc2ea9)

Cuando entremos en el servidor con Google Chrome, hacemos clic en crear cluster.

![image](https://github.com/user-attachments/assets/2ab61166-d24a-48ba-95fd-b9f11f3e0287)

Y para unir al otro pve, hacemos clic en información del cluster.

![image](https://github.com/user-attachments/assets/3432b1d8-2a65-464b-bb3c-643abbb7a7f1)

Y copiamos la información.
A la hora de unir el pve2, nos pide la información del cluster, la ip, y la contraseña, por lo tanto la información la copiaremos en su campo correspondiente, y lo demás según se haya configurado.

![image](https://github.com/user-attachments/assets/3349783a-e148-45eb-bd78-59d0bf41cb57)

### Comando paravirtualización

Para dar permiso a las máquinas virtuales de acceso a las instrucciones del procesador, entraremos en el PowerShell e introduciremos el siguiente comando:

```
Set-VMProcessor -VMName proxmox -ExposeVirtualizationExtensions $true
```

---

## Amacenamiento compartido

### Creación máquina virtual con servidor NFS

En cuanto a la máquina virtual que vamos a crear a continuación, necesitamos una versión de Ubuntu Server con aproximadamente 512 MB de RAM.

![image](https://github.com/user-attachments/assets/9ef99a60-347e-4755-9b36-3fc5323a9c35)

Le ponemos como adaptador de red el Default Switch para que tenga acceso a internet.

![image](https://github.com/user-attachments/assets/f2e8f393-b53d-4830-88f5-34c430144f8e)

Introducimos la iso de ubuntu en la máquina virtual

![image](https://github.com/user-attachments/assets/745bed98-adf3-4e2f-97bb-c76e7db98950)

### Instalación servidor NFS

Después de configurar el server y elegir que la versión de Ubuntu tiene que ser la minimizada, creamos el usuario

![image](https://github.com/user-attachments/assets/90b57834-bbe6-4f88-a108-4b385e201c46)

Una vez tenemos la máquina configurada y con el ubuntu server instalado, nos disponemos a crear y configurar el servidor NFS
Lo primero que haremos será ejecutar el siguiente comando para actualizar el sistema.

```
sudo apt update && sudo apt upgrade -y
```

Después de actualizar el sistema, nos instalaremos el servidor NFS mediante el siguiente comando:

```
sudo apt install nfs-kernel-server -y
```

![image](https://github.com/user-attachments/assets/898b1b38-ef4a-4512-b2d9-4ea8379badfb)


![image](https://github.com/user-attachments/assets/02afb049-6101-4aa3-9791-e0a78823d982)

Y ya tendremos el servidor NFS instalado

![image](https://github.com/user-attachments/assets/7de9549b-4f22-4c57-b37c-ac805aa38213)

### Configuración servidor NFS

Una vez innstalado el servidor NFS, tenemos que configurarlo, y para ello tendremos que crear un directorio para compartir

```
sudo mkdir -p /mnt/nfs_share
```

Y le estableceremos los permisos al directorio previamente creado para que se pueda acceder a él mediante los siguientes comando (en orden)

```
sudo chown nobody:nogroup /mnt/nfs_share
```

```
sudo chmod 777 /mnt/nfs_share
```

Editamos el archivo `/etc/exports` y añadiremos la siguiente línea 

```
/mnt/nfs_share 172.23.42.121/24(rw,sync,no_subtree_check,no_root_squash)
```

El resultado quedaría de esta manera

![image](https://github.com/user-attachments/assets/88ff53e2-17a9-4059-80de-707d8078234d)

Y reiniciamos el servidor con el siguiente comando:

```
sudo systemctl restart nfs-kernel-server
```

Ahora buscaremos nuestra IP asignada al Default Switch

![image](https://github.com/user-attachments/assets/ed83760d-3ae1-40c0-ab04-5c3089d77184)

Y cuando la tenemos, iniciamos el entorno gráfico de Proxmox, hacemos clic en el almacenamiento y a Agregar

![image](https://github.com/user-attachments/assets/261f50a5-1868-4a27-b980-e200552a4091)

Rellenamos los siguientes campos con el nombre, la IP, el contenido y los nodos del clúster

![image](https://github.com/user-attachments/assets/4e9970c3-97f5-4fff-8b74-4eda15b815e8)

Y ya nos sale el resultado

![image](https://github.com/user-attachments/assets/cbeda5fd-e6e2-437d-aba1-47380dc090d6)

---

## Creación y gestión de contenedores

Antes de crear los contenedores debemos seguir los siguientes pasos:

### Conexión a internet en los contenedores

Para tener conexión a internet en los contenedores tendremos que seguir los siguietes pasos.
Entramos en el archivo `/etc/network/interfaces` con **nano** y al final del archivo agregamos lo siguiente:

```
auto vmbr1
iface vmbr1 inet static
    address 192.168.100.1
    netmask 255.255.255.0
    bridge_ports none
    bridge_stp off
    bridge_fd 0
    post-up echo 1 > /proc/sys/net/ipv4/ip_forward
    post-up iptables -t nat -A POSTROUTING -s '192.168.100.0/24' -o vmbr0 -j MASQUERADE
    post-down iptables -t nat -D POSTROUTING -s '192.168.100.0/24' -o vmbr0 -j MASQUERADE

```

Después editaremos el archivo `/etc/sysctl.conf` y descomentaremos la línea que dices:

```
net.ipv4.ip_forward=1
```

Y aplicamos los cambios con los siguientes comandos 

![image](https://github.com/user-attachments/assets/dec32205-a013-4a1b-b3b0-db64a0620444)

### Contenedor 1

Para crear los contenedores, nos tendremos que dirigir a la parte superior derecha de nuestra pantalla en donde dice "Crear CT", los pasos que se realizarán son los siguientes.

#### General

![image](https://github.com/user-attachments/assets/98e156f1-aaf6-440c-ba60-4a73873f69d2)

#### Plantilla

Seleccionamos la plantilla que hemos descargado anteriormente (Debian)

![image](https://github.com/user-attachments/assets/820988f7-7078-46aa-865b-db6bc2c67d47)

#### Discos

Aquí hacemos clic sobre siguiente ya que no hay nada que configurar

![image](https://github.com/user-attachments/assets/31cc3cf4-2804-4782-b0ce-65dd07084dba)

#### Núcleos

Le ponemos un núcleo

![image](https://github.com/user-attachments/assets/fb1cd5fb-b61c-4893-a1af-bb50faa3667b)

#### Memoria

En cuanto a la memoria le asignaremos medio GB de RAM (512 MB)

![image](https://github.com/user-attachments/assets/37a836e9-a8f9-45f3-97fb-4c6d917bd90a)

#### Red

Esta parte es muy importante, al puente le asignaremos el vmbr1, que es el que creamos anteriormente, y en la IPv4 le asignamos una que deseemos, junto a su gateway (puerta de enlace)

![image](https://github.com/user-attachments/assets/45911ce7-845b-423e-9380-ecd21460a440)

#### DNS

Los dejamos en vacío

![image](https://github.com/user-attachments/assets/6e5d2ac8-e09f-44fc-9e2f-65c7aaa4d043)

Una vez realizada la creación del contenedor, comprobamos que tenga internet mediante el siguiente comando 

![image](https://github.com/user-attachments/assets/04923c9c-4f0c-43ac-a32f-a83083fc8acd)

Y ahora instalaremos el servicio que nos pide el enunciado de la práctica.

![image](https://github.com/user-attachments/assets/54e8aab6-5516-4dce-a4ce-433f77c660ac)

![image](https://github.com/user-attachments/assets/46a0f934-0571-4f4e-816d-4f0dc1266b30)


### Contenedor 2

Tendremos que repetir la configuración que realizamos en el nodo 1, pero esta vez en el 2 (pve2)
Una vez lo hayamos configurado, entraremos en la creación del contenedor (arriba a la derecha de la pantalla). Seguimos los siguientes pasos:

#### General

Introducimos el ID del contenedor junto con la contraseña

![image](https://github.com/user-attachments/assets/c15b9d84-de04-4a57-b30e-595521a4d934)

#### Plantilla

Seleccionamos la plantilla del Debian

![image](https://github.com/user-attachments/assets/de1a52be-8c5b-4441-85c4-69c80c474883)

#### Discos

![image](https://github.com/user-attachments/assets/0290a146-fb81-4857-8e30-2efa4baf3f52)

#### Núcleos

Asignamos un núcleo al contenedor

![image](https://github.com/user-attachments/assets/fc6f353b-1dcb-49e6-b247-0a4c993f6b20)

#### Memoria

Asignamos 512 MB de RAM

![image](https://github.com/user-attachments/assets/03a429a5-4948-487b-9ee3-e9c4f48477cd)

#### Red

Le establecemos el puente que creamos anteriormente, junto con la IP que queramos y su gateway

![image](https://github.com/user-attachments/assets/12e4a003-7fb2-4128-96b6-1c97930c75e3)

#### DNS

![image](https://github.com/user-attachments/assets/b42f9136-443f-4d3b-bc69-1d1b49c2ec1e)

Instalamos el servidor que se nos pide (SSH)

![image](https://github.com/user-attachments/assets/3433104d-0efb-4c89-b56f-0438184da7bf)

Lo activamos y lo iniciamos

---

## Creación y gestión de máquinas virtuales (contenedores porque las máquinas no tienen conexión)

### Contenedor 1

Realizamos la creación del primer contenedor, en el primer nodo, en el que le introduciremos la siguiente configuración

![image](https://github.com/user-attachments/assets/0630e8dd-1948-411d-8627-e0227628d2e0)

Ahora que tenemos internet en el contenedor, instalamos un servicio, como por ejemplo el SSH. Primero actualizamos el sistema con:

```
apt update
```

![image](https://github.com/user-attachments/assets/189c2d9c-1a61-4f3e-a774-efdeaaa6862a)

Y después instalamos el servicio que nos solicita la práctica con:

```
apt install -y nginx
```

![image](https://github.com/user-attachments/assets/067271d5-5397-434a-8eda-9700747e1058)

### Contenedor 2 (nodo 2)

Seguiremos el mismo proceso que en el primer contenedor del nodo 1.
Creamos el contenedor introduciendole la siguiente configuración.

![image](https://github.com/user-attachments/assets/f851ef7f-f29f-4f39-8e0e-3f1b4fd22fa8)

Ahora iniciamos el contenedor y lo actualizamos

![image](https://github.com/user-attachments/assets/6e92cde1-faf3-4da8-9cd8-231d0f56c549)

Y ahora instalaremos un servicio.

![image](https://github.com/user-attachments/assets/36498b5b-5501-410d-b6e0-2a364fb3d782)

---

## Migración de máquinas (contenedores)

### Creación del contenedor

Realizamos el proceso de creación de un contenedor, en el primer nodo.

![image](https://github.com/user-attachments/assets/6a46d411-622e-4bd7-8673-852dd777db07)

### Migración en caliente

Como nos indica el apartado, tendremos que inciar el contenedor e instalar un servicio que funcione en el contenedor, por lo tanto seguiremos los pasos de instalación de servicios del apartado anterior.

![image](https://github.com/user-attachments/assets/51f85848-1598-4f28-a2f5-a249bab049fa)

E instalamos el servicio

![image](https://github.com/user-attachments/assets/896d1c58-0c44-4589-ab63-ee54bac1e683)

Y ahora mientras el contenedor está iniciado, hacemos clic sobre migrar

![image](https://github.com/user-attachments/assets/066b32d1-d41f-4168-b184-a863a28cba68)

Confirmamos que se migre al nodo 2

![image](https://github.com/user-attachments/assets/24a9466f-3d70-4398-bbf2-746adf034c32)

![image](https://github.com/user-attachments/assets/095f5b39-0a92-4213-8cf1-6287d8f0e1fd)

### Migración en frío

Con el contenedor apagado, realizamos los pasos anteriores, clic derecho sobre él y seleccionar migrar.

![image](https://github.com/user-attachments/assets/ef1bf291-a745-4340-9f6c-2231a2ab5b0a)

El contenedor está apagado, por lo tanto realizamos la migración haciendo clic en el botón que dice "Migrar"

![image](https://github.com/user-attachments/assets/6a1db5d1-ed04-491b-95cd-b60d439f4b07)

---

## Creación del backup 

### Creación contenedor

Creamos un contenedor con un servicio. Primero seguimos el proceso de creación.

![image](https://github.com/user-attachments/assets/a528f32d-4918-4996-a80c-55fc72acbfd6)

Instalamos el servicio que deseemos (por ejemplo nginx)

![image](https://github.com/user-attachments/assets/34b54807-de17-4c1f-8b17-0cdad3cba6c1)

Ahora, seleccionaremos el contenedor y hacemos clic sobre la pestaña de Respaldo.

![image](https://github.com/user-attachments/assets/8153d8a6-7085-45eb-bcb7-80e7829a4243)

Respaldar ahora.

![image](https://github.com/user-attachments/assets/e8d7ec13-a95e-4d48-aed0-a7f181f0a8f8)

Tenemos ya el backup

![image](https://github.com/user-attachments/assets/7cacf896-3268-41fb-8ee6-3226e80304c7)

Ahora eliminamos el contenedor en el que está creado el backup

![image](https://github.com/user-attachments/assets/fe306e8b-67e0-4b3f-8218-ec78f7d32649)

Y la copia de seguridad está en el almacenamiento del nodo que seleccionamos

![image](https://github.com/user-attachments/assets/c65f42a5-a6a2-4f38-bca6-36b9a0769fee)

Hacemos clic en restaurar y recuperamos el contenedor

![image](https://github.com/user-attachments/assets/8fc2d9c1-eedd-49f4-9dcf-d567267cea16)

![image](https://github.com/user-attachments/assets/ca719256-1d07-4150-b5e0-802ab197ec65)

### Backups automáticos

En el clúster seleccionamos la pestaña de Respaldo y hacemos clic en Agregar

![image](https://github.com/user-attachments/assets/4889210a-ed26-488f-820c-60fcc9ef72ef)

Rellenamos los campos con la configuración que deseemos y el/los contenedor/es que queramos.

![image](https://github.com/user-attachments/assets/d498eb2a-fef4-492e-b184-0fe02b810298)
