
# DOCUMENTACIÓN PROYECTO PFSENSE

## Creación de la máquina virtual de pfSense

En `VirtualBox`, en el desplegable de "Taller firewall" pulsaremos el clic derecho y seleccionaremos "Nueva máquina"

![image](https://github.com/user-attachments/assets/8c68be30-1044-489e-9baa-f9fb82a40072)

Primero le damos el nombre que deseemos a la máquina y le pondremos la ISO de pfSense, que es otorgada en los archivos de la práctica.

![image](https://github.com/user-attachments/assets/49e8771d-b044-4e01-bede-33ffa9dc739e)

Posteriormente, para que arranque la máquina, es necesario que configurar el tipo y cambiárselo a BSD.

![image](https://github.com/user-attachments/assets/749e9d75-8f56-4d44-a61d-dd414bcc1979)

De esta manera tendremos la máquina con el pfSense.

Dentro del Taller de firewall tendremos 3 máquinas:
- Máquina de la Web del servidor
- Máquina con Kubuntu, para acceder al servidor de pfSense
- Máquina con la ISO de pfSense

## Configuración de las redes en la máquina virtual de pfSense (DMZ Y LAN)

Dentro de `VirtualBox` en las Herramientas (parte superior izquierda de la pantalla) hacemos clic en los 3 puntos.

![image](https://github.com/user-attachments/assets/fc5ff544-fd5e-400e-ac99-ce88d4450ded)

Posteriormente, tendremos que hacer clic sobre Red

![image](https://github.com/user-attachments/assets/87fbb9c6-764d-4859-a42e-540937cf824c)

Y ahora dentro de las **Redes NAT** tendremos que crear dos:
- Red **DMZ**
- Red **LAN**

Al hacer clic derecho sobre la pantalla, tenemos la opción de Crear, para añadir una red.

![image](https://github.com/user-attachments/assets/9ddf16b7-f763-4542-8b66-84bc21759d99)

Seleccionamos esa opción y se nos crea una red, a la que tendremos que darle, por ejemplo, el nombre de DMZ.

En cuanto a la IP, es la que nos da la práctica (192.168.20.0/24) y tendremos que deshabilitar el DHCP para que no se estblezca una IP por defecto a dicha red.

La configuración de la red debería quedar de esta manera:

![image](https://github.com/user-attachments/assets/f0d15fb8-726c-4840-bfed-47625c9df5e8)

En cuanto a la LAN seguiremos el mismo procedimiento que en la DMZ, pero asignándole otra IP, que recibimos del enunciado de la práctica (192.168.10.0/24)

![image](https://github.com/user-attachments/assets/d32555aa-fd8c-47ef-b945-4dee4f896e38)

De esta manera crearemos las redes necesarias para la máquina virtual.

## Asignación de las redes en la máquina virtual del pfSense

Para asignar las redes que acabamos de crear, tendremos que seleccionar la **configuración** de nuestra máquina en la que tenemos el pfSense.

![image](https://github.com/user-attachments/assets/73adf406-c1bb-405b-baa1-cd9600874336)

Dentro de la configuración nos vamos a la parte de Red, en la que encontraremos los adaptadores de red de la máquina virtual.

![image](https://github.com/user-attachments/assets/7c1959ef-0863-47e4-8351-30e9914a7b64)

#### Primer adaptador

En cuanto al primer adaptador le pondremos que esté conectado a la NAT

![image](https://github.com/user-attachments/assets/ae71e120-a14d-441b-9c47-a31bdb86e508)

#### Segundo adaptador

Para el segundo adaptador de red, tendremos que indicarle que esté conectado a una red NAT, y posteriormente le asignaremos el nombre de dicha red (en este caso será la LAN).

![image](https://github.com/user-attachments/assets/9b93c416-80ff-4f58-8cff-e0663c6d2f52)

#### Tercer adaptador

La configuración establecida en el tercer adaptador de red es muy parecida a la del segundo, primero le indicaremos que esté conectado a una red NAT y posteriormente le asignaremos el nombre de la red (en este caso es la DMZ)

![image](https://github.com/user-attachments/assets/0563c337-d4c6-4452-929e-4f2155497d2f)


## Instalación de pfSense en la máquina virtual

Después de realizar todos los pasos anteriores, nos disponemos a iniciar la máquina con la iso de pfSense.
La primera pantalla que nos aparece es la siguiente:

![image](https://github.com/user-attachments/assets/309127e4-a616-430a-9f2a-00a30bc59242)

En los siguientes pasos tenemos que aceptar todo:

![image](https://github.com/user-attachments/assets/efc9deb3-a210-43ac-8fd8-cc9135d84b65)
![image](https://github.com/user-attachments/assets/942f67bc-84b1-4063-a9e3-0c83e9a22e24)
![image](https://github.com/user-attachments/assets/f52cd541-a858-47c3-ac9e-2c3938f66e1f)
![image](https://github.com/user-attachments/assets/0a1e5ac1-f5c4-4dce-9fa1-fe9b8517e025)

Hasta esta, en la que debemos seleccionar el disco en el que queremos que se nos instale el pfSense.

![image](https://github.com/user-attachments/assets/6e0c25ef-94d1-44b2-bd83-776eb6c704eb)

Aceptamos y debemos esperar a que se instale y posteriormente reiniciar.

![image](https://github.com/user-attachments/assets/dfbc2781-c909-43cd-bcae-0a28c513151d)
![image](https://github.com/user-attachments/assets/30af8e72-5dcb-4bed-a87c-e9af83e9c1d6)
![image](https://github.com/user-attachments/assets/a1119176-f433-4bd0-b90a-af0576e21b1f)

Dentrod el servidor tenemos que seguir los pasos que nos indican las capturas de la práctica.

![image](https://github.com/user-attachments/assets/89c04d7d-8b9b-46c6-b4a8-bd0305f4c2bc)

Escribimos 2 para asignar las interfaces.
Volvemos a escribir 2 para configurar la interfaz LAN y le decimos que no queremos que se configure la LAN con DHCP, ya que queremos establecer nuestra IP (192.168.10.1)

![image](https://github.com/user-attachments/assets/794ec1f2-26cf-48ff-bee5-c65e3fd46070)

Le establecemos la máscara de subred 255.255.255.0 y le decimos que no queremos comfigurar la IPv6 de la LAN con DHCP6.
Posteriormente le decimos que queremos activar el server DHCP en la LAN para establecerle un rango que será de la IP 192.168.10.100 a la 192.168.10.200.
Finalmente diremos que no queremos vovler a utilizar el HTTP como protocolo de la web.

![image](https://github.com/user-attachments/assets/b14471ef-d917-4d49-b75d-e227d52f4d59)

Y tendremos el servidor creado.

![image](https://github.com/user-attachments/assets/b1e8a8a3-b39e-4950-8ea0-37d5dcb1087a)

Dentro de la máquina de Kubuntu introducimos la ip del servidor en el bsucador y nos llevará a la siguiente página:

![image](https://github.com/user-attachments/assets/a2977d60-226e-46a9-a31c-f7bd3a2bc4ea)

Escribimos el nombre de usuario: **admin**.
Y en cuanto a la contraseña, si es la predeterminada, será: **pfsense**

![image](https://github.com/user-attachments/assets/43453937-1a18-4bbd-a31b-9ec0abe8f5d2)

Una vez dentro de pfSense, nos introduciremos nos dirigimos a Interfaces > Assignments y añadiremos la interfaz OPT1.

![image](https://github.com/user-attachments/assets/0c2cd9c7-ea39-483d-b9ca-d343bf45f0e9)

Hacemos clic sobre "Save" para que se guarden los cambios.

![image](https://github.com/user-attachments/assets/c5a101d3-62d1-4ea3-9d92-82a272fe3dba)

Ahora nos dirigiremos al apartado de Interfaces > OPT1 para configurarlo

![image](https://github.com/user-attachments/assets/636ca6f2-1d44-4624-bd78-9a2e827e4f8a)

En el apartado de "IPv4 Configuration Type" desconfiguramos el None y seleccionamos Static IPv4, además de activar la 
checkbox de Enable interface.

![image](https://github.com/user-attachments/assets/03469770-fac6-428c-8f62-afcb4ddc1d29)

En la sección de Static IPv4 Configuration en el cuadro de texto de IPv4 Address pondremos la de la interfaz DMZ 
(192.168.20.1). Además, justo a la derecha nos encontramos con un desplegable de números, seleccionaremos el 24 (por la 
máscara de subred).

![image](https://github.com/user-attachments/assets/fa98618f-863a-4b47-b2e0-15cd6fe96a36)

Y guardamos para que los cambios se ejecuten.

![image](https://github.com/user-attachments/assets/df856976-a82c-432e-9c4d-979ea5aa0c5e)

Ahora nos iremos a la parte de DHCP Server (Services > DHCP Server)

![image](https://github.com/user-attachments/assets/35ba137a-c368-4cfb-b20e-49de64fe6534)

Configuramos la LAN, en la que activaremos el server DHCP para dicha interfaz.
En el desplegable de Deny Unknown Clients seleccionamos "Allow all clients"

![image](https://github.com/user-attachments/assets/8aa40d2f-b94e-48ec-b35a-8468278471fb)

Posteriormente, seguiremos la ruta de System > Advanced, que se encuentra en la parte superior de nuestra pantalla.

![image](https://github.com/user-attachments/assets/1f6c6fb6-97f6-4055-9979-358d1d3f6a6c)

Dentro de los ajustes avanzados del sistema, nos posicionaremos en la parte de Networking.

![image](https://github.com/user-attachments/assets/160b5fda-5f12-4601-ba4c-72211adab122)

En las opciones de DHCP, en cuanto al Server Backend seleccionamos el Kea DHCP.

![image](https://github.com/user-attachments/assets/8889ef2a-6d88-4765-a22a-6927e78d8f27)

Y guardamos los cambios.

![image](https://github.com/user-attachments/assets/0436ef02-75c3-4894-9b5d-7ad595f77809)

Ahora volvemos a configurar el server DHCP para la interfaz DMZ .

![image](https://github.com/user-attachments/assets/60a1a3c4-0157-46d2-b12f-2617826fcde5)

Seleccionamos la DMZ esta vez, y le activamos el server DHCP en la interfaz, además permitimos que cualquier DHCP obtemga 
una IP dentro del rango que le pondremos a continuación.

![image](https://github.com/user-attachments/assets/926e3fb4-0194-4b10-9c86-63e10a481376)

Le establecemos el rango de 192.168.20.100 y 192.168.20.200

![image](https://github.com/user-attachments/assets/f1fbe1d5-82dc-4879-8aca-48a5d5a74110)

Y finalmente en cuanto a los servidores DNS (al final del todo), en el primero le introducimos el 8.8.8.8 (perteneciente a 
Google) y el 8.8.4.4 (también de Google)

![image](https://github.com/user-attachments/assets/4e521f6c-9370-4b6c-bc0e-979164e8cb7f)

---

# CREACIÓN DE REGLAS

## Regla NAT

Esta regla consiste en permitir el acceso al puerto 80 del servidor web desde el exterior de la red LAN.
Para ello, primero nos dirigimos a Firewall > NAT, y en el apartado de Port Forward hacemos clic sobre Add, para añadir una 
regla nueva.

![image](https://github.com/user-attachments/assets/ab13e0ec-a252-40ee-832f-73cdb2546d92)

En cuanto a la Interfaz, le colocaremos la LAN, y el protocolo será TCP. 
El destino será una dirección LAN y en cuanto al rango de puertos de destino seleccionamos en ambas el puerto HTTP (puerto 
80).

![image](https://github.com/user-attachments/assets/98d3fee1-0610-4e5c-b3da-747fd1e99dee)

La siguiente configuración será la siguiente.
La IP de destino se redirigirá a la red DMZ y el puerto de destino será el HTTP (puerto 80).
En la descripción pondremos lo que deseemos. 

![image](https://github.com/user-attachments/assets/f79cdbfd-7bdf-45a0-bda6-46b576586ae0)

Guardamos los cambios.

![image](https://github.com/user-attachments/assets/8591230c-1af7-4d89-bd95-c42e5a0f81a3)

## Regla SSH

Para esta regla debemos ir al mismo sitio que para la anterior y seleccionar Add.
Esta vez la interfaz será en WAN, y el protocolo seguirá siendo TCP. El destino será una red WAN y el rango de destino será el SSH (puerto 22).
En cuanto al campo de Redirect target IP, le agregaremos la 192.168.10.10.

![image](https://github.com/user-attachments/assets/1827f644-0521-4f41-bed7-45067dd7ce16)

Se redirigirá al puerto 22 (SSH).
En la descripción escribiremos algo relacionado con lo que hemos creado.

![image](https://github.com/user-attachments/assets/4711db3c-3240-448b-9fbf-f3a2f53c9317)

Escribimos en la terminal de la máquina Kubuntu el siguiente comando.

```
ssh estudiante@192.168.10.2
```


## Regla para bloquear el acceso SSH al servidor web desde cualquier sitio

Esta regla hará que cualquier IP que intente conectarse al servidor web, sea bloqueada.
Esta vez nos dirigimos a la ruta Firewall > Rules > DMZ.

![image](https://github.com/user-attachments/assets/2ccd6f8f-b88f-47a7-9158-46587af71d4c)

La acción que hará será bloquear el acceso al servidor web.
La interfaz será la DMZ y el protocolo TCP.

![image](https://github.com/user-attachments/assets/ff7b4f5b-e8ea-47d1-84d3-452dc7d27149)

El campo de source lo dejamos en Any, para que cualquier intento sea bloqueado por esta regla, en el puerto 22, que es el 
que pertenece al SSH.

![image](https://github.com/user-attachments/assets/ca3c25e0-6b91-4b11-bb54-2cf4729ff572)

En cuanto al destino, escribimos la IP de nuestra máquina y el puerto del 22 (SSH) hasta cualquier otro.
En la descripción escribimos cualquier texto relacionado con la regla, como es mi caso.

![image](https://github.com/user-attachments/assets/75c58917-4317-43fe-bd17-38091157d27f)

---

## Regla para dejar pasar a la ip de nuestro equipo

Esta regla lo que hará es dejar pasar una IP que le introduzcamos, nos situamos en el sitio que nos indica siguiendo la ruta Firewall > Rules > DMZ (la misma que la regla anterior).

![image](https://github.com/user-attachments/assets/50eeb04a-3169-4e1e-b8ec-055dce677c0d)

Aquí colocamos la IP de nuestra máquina ya que es la que esta regla dejará pasar.

![image](https://github.com/user-attachments/assets/51caa2e4-2b8a-41a3-833f-bc8f6034d5ed)

En cuanto al destino, colocaremos también la IP de nuestra máquina, y el puerto 22.

![image](https://github.com/user-attachments/assets/41fe125a-3d2c-4d2d-8dda-d602268126cf)

En la terminal del Kubuntu escribimos lo siguiente.

```
ssh estudiante@192.168.20.100
```

Y nos sale lo siguiente

![image](https://github.com/user-attachments/assets/86aa1d58-f62e-49fd-aa77-0dbe0e753ba7)

Esto significa que estamos en el servidor web desde el SSH.

# TAREAS OPCIONALES

## Regla de Firewall para tener acceso al servidor web solo de 8:00 a 20:00.

### Creación horario

Entramos en el apartado de Firewall y seleccionamos Schedules.
![image](https://github.com/user-attachments/assets/1824d358-d99d-4dbe-9e0b-df3b934a270d)

Hacemos clic en Add para añadir el horario.
![image](https://github.com/user-attachments/assets/431e5eff-b2ed-4dc6-b3c0-8567a359b408)

Primero introducimos el nombre que deseemos del horario junto a su descripción.
![image](https://github.com/user-attachments/assets/ef9c2527-fcab-4c86-82ea-31e03be15893)


Seleccionamos lo que queda del mes de mayo para realizar la regla.
![image](https://github.com/user-attachments/assets/7e0fe8d3-7fea-4bcf-a381-298b28cde7b7)

Justamente debajo del calendario que nos da el PfSense, encontramos un campo para establecer las horas.
![image](https://github.com/user-attachments/assets/23ea240c-4460-4eaa-91f4-7251c1180460)

Aquí confirmamos que nuestro horario está bien configurado y lo guardamos.
![image](https://github.com/user-attachments/assets/5b60eb48-6798-4efe-9ed1-50a2aaffb71e)

Confirmamos que se nos ha guardado después de hacer clic sobre el botón de "Save".
![image](https://github.com/user-attachments/assets/29fcb411-b8b1-494e-b266-139635c4c7a5)

### Creación regla con el horario

Una vez tenemos creado el horario para bloquear el acceso al servidor web, procedemos a crear la regla que nos permita hacer ese bloqueo.
Nos dirigimos a `Firewall > Rules > DMZ` y hacemos clic en Add, para añadir dicha regla.


![image](https://github.com/user-attachments/assets/6c0ac2f2-08de-4c53-b1a8-a1616391817e)

Visualizamos las opciones avanzadas para ver la opción de Schedule, en la que introduciremos el horario que creamos anteriormente.

![image](https://github.com/user-attachments/assets/7b6e3718-d8b7-4f2d-823e-02fd5508f9b5)

Y ahora le introducimos la descripción que deseemos.

![image](https://github.com/user-attachments/assets/ada1aa5a-c615-4371-a759-169c086987a0)

Para verificarlo, en la siguiente imagen se puede apreciar que la hora pertenece al rango de horas entre las 8:00 y las 20:00, por lo tanto me deja acceder al servidor web.

![image](https://github.com/user-attachments/assets/3e0881ce-511a-48fd-97f5-d4c80b272575)

---

## CARP (Common Address Redundancy Protocol)

El CARP es un protocolo  de red que permite que varios hosts de la misma red compartan una única dirección IP. 
Esto se utiliza principalmente para proporcionar alta disponibilidad, lo que significa que si un host falla, los demás pueden tomar el control sin interrumpir los servicios de red.

En cuanto al pfSense permite que dos o más firewalls pfSense compartan una IP virtual (VIP) y así proporcionen alta disponibilidad. Si el firewall maestro falla, el secundario asume automáticamente el control de la red usando esa misma IP virtual.

### Requisitos 

#### Dos dispositivos pfSense

Dos dispositivos en los que esté instalado el pfSense independientemente en máquinas virtuales.
Deben esar configurados en 2 interfaces de red diferentes:
- Uno para LAN
- Otra para WAN

#### Direcciones IP 

Para cada interfaz se necesitan las siguientes IPs:
- Para el nodo maestro (Firewall principal)
- Para el nodo secundario (Firewall secundario)
- IP Virtual

#### Sincronización entre los sistemas

- Dentro del nodo maestro de pfSense (Firewall principal), debe tener acceso a `System > High Availability`
- Y se debe configurar la IP del nodo secundario (Firewall secundario), junto al usuario y contraseña
- Activar sincronización de reglas

#### Red 

- La WAN y la LAN deben estar conectadas a ambos sistemas pfSense. 
- La dirección IP virtual debe estar en el mismo segmento de red que la dirección IP del sistema principal. 
- Los sistemas deben estar en la misma subred para la monitorización de CARP.

#### Versiones de pfSense

Es fundamental que ambas máquinas tengan la misma versión de pfSense instalada para que no den errores.


# TRABAJO FOL (IPE)

