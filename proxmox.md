# DOCUMENTACIÓN PROYECTO PROXMOX (TEDRA)

## Creación de máquinas virtuales

Para realizar la siguiente práctica, debemos crear dos máquinas virtuales con la iso de Proxmox y con una capacidad de 6144 MB de RAM.
Para encontrar la iso de Proxmox, realizamos una búsqueda en el buscador de Google, y en la página oficial de Proxmox encontramos en la parte superior derecha, el apartado de `Downloads`

![image](https://github.com/user-attachments/assets/9a44e973-3701-4df2-bed6-b31c6db1a606)

Hacemos clic en Downloads y descargamos el primer archivo, que es la iso de **Proxmox**.

![image](https://github.com/user-attachments/assets/ddb86622-596c-4810-9048-f374cd449c60)

Una vez instalada, nos dirigimos a Hyper-V para crear las máquinas virtuales.
Le ponemos la memoria RAM que nos indiica el enunciado de la práctica

![image](https://github.com/user-attachments/assets/ed7274d0-fac3-4ec2-be5e-3c1b6c5b9356)

Y después le introducimos la iso que descargamos anteriormente.

![image](https://github.com/user-attachments/assets/04f6fb7c-9001-4bc2-9c29-7d69413a1e4f)

Una vez realizados estos pasos, la ejecutamos.

![image](https://github.com/user-attachments/assets/eee7cb9a-d636-435d-ae42-a22d5bc9d283)

Después de ejecutarla, nos sale la siguiente pantalla, en la que elegiremos la instalación desde el entorno gráfico.

![image](https://github.com/user-attachments/assets/cc49404f-8ddb-4ffa-9fc0-9d8a0da46ba7)

Aceptamos los términos de licencia

![image](https://github.com/user-attachments/assets/fbf4baa4-6489-4f5d-b628-ff00fb4a3adf)

Seleccioanamos el ext4

![image](https://github.com/user-attachments/assets/4fe19bc1-3b0d-40d9-a0f5-b1376f02372f)

En cuanto al idioma, seleccionaremos España y la zona horaria de Madrid 

![image](https://github.com/user-attachments/assets/dfc2649d-99dd-481f-bbfd-e2eed6712b14)

Introducimos nuestra contraseña y nuestro correo eléctronico

![image](https://github.com/user-attachments/assets/786c5f10-9c5e-49fa-97c6-ce82eae4d21d)

Introducimos las IPs y le damos a "Next"

![image](https://github.com/user-attachments/assets/b855190a-5df5-411c-9f35-c7e5dbca2772)

Procedemos con la instalación.

![image](https://github.com/user-attachments/assets/61e14be5-4986-4954-8a4f-3edb411f4198)

Cuando se ha instalado, entramos en la configuración de la máquina y eliminamos la iso de proxmox:

![image](https://github.com/user-attachments/assets/fb168ec7-7cd9-498b-bc62-a7a41deef349)

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

Ahora crearemos la otra máquina con la iso de Proxmox.

