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

En cuanto a esta pantalla, crearemos una galería vertical para que se muestren los empleados que hay en la empresa de recursos humanos.
La galería vertical la encontramos arriba a la izquierda en Insertar.

![image](https://github.com/user-attachments/assets/528fd158-9ad8-4af6-8021-a3563d7b9302)

Le introducimos los datos de la tabla 1 ya que es la que recoge los datos de los empleados de nuestra empresa.
Hacemos clic sobre nuestra galería y en la parte de Datos, podemos obtener el listado de empleados.

![image](https://github.com/user-attachments/assets/34090fd9-dc14-4189-8570-158443ece7d0)

