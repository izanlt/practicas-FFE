# DOCUMENTACIÓN PROYECTO POWER APPS

## INTRODUCCIÓN
Este proyecto trata de crear una aplicación en Power Apps para gestionar una empresa de recursos humanos. Los datos serán introducidos a través de un documento de Excel que tendrá
diferentes tablas que se irán insertando según la pantalla en la que nos situemos.

En la aplicación tendremos pantallas que su función será redirigir al usuario, otras para crear o modificar empleados, entrevistas, eventos...
o bien para eliminar los mismos.

## Introducción de los datos a la aplicación
Los datos se agregan mediante un docuemnto de Excel (en este caso), por lo que tendremos que hacer la creación de las tablas y la introducción de los datos en las mismas.
Cuando tengamos el documento listo, en Powwer Apps tendremos que crear una aplicación a partir del Excel

Para introducir los datos del Excel tendremos que hacer lo siguiente:
Clic en Agregar datos > Buscamos "Excel (Empresa)" > Documentos > Documento de Excel que deseemos.
Una vez hecho eso, agregamos las tablas.

![Captura valores 1](/imagenes/capexcel.jpg)

## Creación del índice (Primera pantalla)
Para la creación del índice tendremos que crear una pantalla en blanco con los botones que correspondan a los apartados a los que queramos ir.
Los botones están en la parte de Insertar arriba a la izquierda del menú de Power Apss.

![image](https://github.com/user-attachments/assets/ea9f36ff-9a78-4459-8491-90342a7f1b82)

En mi caso, el indice me quedó de esta manera:
![Captura valores 1](/imagenes/indice.jpg)

Para crear el redireccionamiento a las distintas pantallas, en el apartado de las fórmulas de cada botón escribiremos lo siguiente:
```
Navigate(PantallaDeseada)
```

## Creación pantalla de creación, modificación y eliminar empleados (Segunda pantalla)

El resultado de la pantalla es el siguiente:

![image](https://github.com/user-attachments/assets/10ad716b-7a06-49fe-a4ab-7aafdbe3fc23)


En cuanto a esta pantalla, crearemos una galería vertical para que se muestren los empleados que hay en la empresa de recursos humanos.
La galería vertical la encontramos arriba a la izquierda en Insertar.

![image](https://github.com/user-attachments/assets/528fd158-9ad8-4af6-8021-a3563d7b9302)

Le introducimos los datos de la tabla 1 ya que es la que recoge los datos de los empleados de nuestra empresa.
Hacemos clic sobre nuestra galería y en la parte de Datos, podemos obtener el listado de empleados.

![image](https://github.com/user-attachments/assets/34090fd9-dc14-4189-8570-158443ece7d0)

En la parte de la derecha de los botones, tenemos 3 que están alineados para realizar cambios en el listado de los empleados con una imagen icónica en cada uno.
Para tener los redireccionamientos o vínculos dentro de la aplicación, la fórmula que tendremos que poner será la siguiente (ejemplo con el primer botón):

```
Navigate('Creacion de Empleados')
```

En cuanto a los botones de "Siguiente" y de "Volver al índice" utilizaremos la misma fórmula que arriba pero con diferentes pantallas.

## Pantalla de creación de empleados
En esta pantalla generaremos empleados en el listado de los mismos.
Esta tiene incluida una tabla a la izquierda en la que nos aparecerán los empleados de la empresa, y a la izquierda, un formulario en el que rellenaremos los datos que se nos solicita.

El resultado de esta pantalla es el siguiente:

![image](https://github.com/user-attachments/assets/1498fc39-9a01-46fc-aabc-a01a921a3bb2)

Arriba a la izquierda creé un cuadro, que se encuenta en parte de "Insertar" y seleccionamos la parte de "Multimedia" para insertar una imagen del ordenador

![image](https://github.com/user-attachments/assets/bd5bda32-36c3-4d63-a937-1a4b4ec0610b)

Para colocar la imagen que queramos, en la parte derecha, donde se encuentran las propiedades del elemento, podemos cargar una imagen para que aparezca en la aplicación.

![image](https://github.com/user-attachments/assets/4a761a28-10cd-45df-8502-935272ce0d40)

## Pantalla de observaciones

Esta pantalla es parecida a la del listado de los empleados y el método de creación es similar.
El resultado es el siguiente:

![image](https://github.com/user-attachments/assets/649688cb-be87-4b16-be66-f25e1ecc39a5)

En este caso solamente hay un botón para la creación de las observaciones.

## Pantalla de creación de observaciones a los empleados

En esta pantalla crearemos una tabla y un formulario en el que podemos crear observaciones a los distintos empleados de la empresa.


