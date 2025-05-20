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

En mi caso, el inicio de la App me quedó de esta manera:

![image](https://github.com/user-attachments/assets/f87bdd5b-1b94-441f-b11f-47c31aa95260)

En cuanto al borde de los botones, debemos seleccionar un botón y en las propiedades irnos hacia abajo del todo, y cambiaremos lo siguiente:

![image](https://github.com/user-attachments/assets/8d214873-318e-4060-a467-1eea43055c59)


Para crear el redireccionamiento a las distintas pantallas, en el apartado de las fórmulas de cada botón escribiremos lo siguiente:
```
Navigate(PantallaDeseada)
```

Cada apartado tiene su respectivo botón con su correspondiente fórmula para que se realice el desplazamiento al campo que se desea.

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

![image](https://github.com/user-attachments/assets/6fb49859-fa50-48cb-83e9-c4582ee56e67)


En este caso solamente hay un botón para la creación de las observaciones.

## Pantalla de creación de observaciones a los empleados

En esta pantalla crearemos una tabla y un formulario en el que podemos crear y eliminar observaciones a los distintos empleados de la empresa.
El código de las fórmulas que se introduce a los botones es el siguiente:

Para el botón de eliminar una observación
```
If(
    !IsBlank(Gallery7.Selected);
    Remove(Tabla1; Gallery7.Selected);
    Notify("Observacion eliminada correctamente"; NotificationType.Success)
)
```
Para el botón de agregar una observación:
```
NewForm(Form3);; UpdateContext({ newMode: true })
```

El resultado de esta pantalla es el siguiente:

![image](https://github.com/user-attachments/assets/13af9ffc-31b0-49e2-b54f-880c5c3b104a)

En la parte de la izquierda de la pantalla creé una galería vertical para ver las observaciones sobre los empleados de la empresa, y por la parte del centro superior, los botones de crear y eliminar observación.

## Pantalla de eventos

En esta pantalla visualizaremos los eventos que tiene nuestra empresa, junto a los empleados involucrados en cada uno de los eventos.
Además, podremos saber cual es el tipo de cada uno de los eventos que se realicen.

El resultado de la pantalla de visualización de eventos es el siguiente:

![image](https://github.com/user-attachments/assets/80d12af5-ed7b-4231-91c2-b6e1bcf1b60d)


Como se puede observar, hay un botón que dice "Crear/eliminar eventos" en el que nos redirige a una pantalla para poder realizar la creación o eliminación de eventos.
Justo al lado, tenemos otro botón que nos lleva a un calendario en el que se recogen los distintos eventos.

## Pantalla de creación y eliminación de eventos

Esta pantalla tiene una galería con el listado de los eventos que la empresa tiene planeados, y a la izquierda 2 botones, uno sirve para crear un evento, y el otro para eliminar.
Las fórmulas que se le escribe a los botones es la siguiente.
Botón de eliminación (la galería se llama "Gallery10"):
```
If(
    !IsBlank(Gallery10.Selected);
    Remove(Tabla4; Gallery10.Selected);
    Notify("Evento eliminado correctamente"; NotificationType.Success)
)
```
Botón de creación:
```
NewForm(Form3);; UpdateContext({ newMode: true })
```

Lo demás, son cambios como la foto del logotipo de la pantalla, o la foto de cada evento.
El resultado es el siguiente:

![image](https://github.com/user-attachments/assets/e0a587ed-4a13-46d2-81e9-4fc7c4452182)

## Pantalla de entrevistas

La creación de esta pantalla consta de una galería situada a la izquierda, teniendo un listado de todas las entrevistas creadas de la empresa.
Los cambios realizados que la diferencian de las demás pantallas son las imágenes utilizadas

![image](https://github.com/user-attachments/assets/e876f603-feb4-4e16-80f8-ba46e958ae1b)

## Creación/Eliminación de entrevistas

Esta pantalla está directamente conectada con la anterior por medio del botón que se visualiza en el centro de la pantalla anterior.
Podemos realizar la creación y entrevistas por medio de los botones en la parte superior del centro, en los que solamente tendremos que seleccionar
la entrevista que deseemos, y al hacer clic sobre el botón de eliminar, la entrevista desaparecerá.

El código de cada botón es el mismo que el de los demás. pero cambiando el nombre de la galería y del formulario.
El resultado es el siguiente:

![image](https://github.com/user-attachments/assets/f86c0924-a551-4c6b-a25f-78afb732e4ef)

## Pantalla de eliminar, dar de alta o baja a un empleado

Esta pantalla está directamente conectada con la segunda (pantalla de empleados) en la que por medio del botón que dice "Dar de baja/alta"
podemos acceder a esta.

Esta pantalla consta de una galería vertical en la que se insertan los datos de la Tabla1 (tabla de los empleados en el Excel) para tener un listado de los mismos.
A la derecha tenemos 3 botones, que todos funcionan de la misma manera: se selecciona el empleado que se desee y al hacer clic en el botón, se realiza la acción que corresponda.

#### Primer botón (Eliminar):

El botón de eliminar tiene el siguiente código en la fórmula para que ejecute la acción deseada:
```
If(
    !IsBlank(Gallery1_1.Selected);
    Remove(Tabla1; Gallery1_1.Selected);
    Notify("Empleado eliminado correctamente"; NotificationType.Success)
)
```
Pongo mi ejemplo, ya que la galería se llama Gallery1_1.

#### Segundo botón (Dar de baja)

La función de ese botón es cambiar el campo llamado "EnPlantilla", en el que al seleccionar un empleado y pulsar dicho botón, se cambia su valor a "No"

![image](https://github.com/user-attachments/assets/d850d9ad-fdac-4a3a-85ff-f8078182905c)

Debajo del nombre y apellidos dice "No", eso significa que no está en plantilla y por tanto el empleado está dado de baja.
El código que necesita el botón es el siguiente:
```
If(
    !IsBlank(Gallery1_1.Selected);
    
    Patch(
        Tabla1;
        Gallery1_1.Selected;
        { EnPlantilla: "No" }
    );

    Patch(
        Tabla2;
        Defaults(Tabla2);
        {
            EmpleadoID: Gallery1_1.Selected.ID;
            Tipo: "Evaluación de salida";
            Fecha: Today();
            Observaciones: "Evaluación realizada correctamente"
        }
    )
)
```

#### Tercer botón (Dar de alta)

Por el contrario al botón de antes, debemos tener un botón para dar de alta a un empleado después de la correspondiente entrevista que tenga.
Si seleccionamos un empleado y presionamos el botón de "Dar de alta", debajo del nombre y apellidos del empleado pondrá "Si"
ya que se habrá cambiado el valor del campo "EnPlantilla", configurádolo de manera que ese empleado está dado de alta.

La fórmula que tiene el botón de dar de alta es la siguiente:
```
If(
    !IsBlank(Gallery1_1.Selected);
    
    Patch(
        Tabla1;
        Gallery1_1.Selected;
        { EnPlantilla: "Si" }
    );

    Patch(
        Tabla2;
        Defaults(Tabla2);
        {
            EmpleadoID: Gallery1_1.Selected.ID;
            Tipo: "Evaluación de salida";
            Fecha: Today();
            Observaciones: "Evaluación realizada correctamente"
        }
    )
)
```

---

El resultado de esta pantalla es la siguiente

![image](https://github.com/user-attachments/assets/68754020-2428-42b4-bddb-3e2b506fec69)

## Pantalla de la ficha de cada empleado.

Esta pantalla está vinculada con la de los empleados, ya que se necesita que se presione el siguiente botón:

![image](https://github.com/user-attachments/assets/9dbdbcac-acc9-4d50-9cc3-24b0e2891040)

El botón que se debe presionar es el que se parece a ">", de esta manera nos redirige a la pantalla de la ficha de cada empleado,
que está formada por una foto de perfil predeterminada y diferentes botones con sus fórmulas.

El resultado de la pantalla es el siguiente (con un ejemplo):

![image](https://github.com/user-attachments/assets/ad7f2592-f547-40a6-b969-e17ff7bbe95f)


Los botones tienen en las fórmulas lo siguiente:

#### Botón nombre

```
empleadoSeleccionado.Nombre
```

#### Botón apellido

```
empleadoSeleccionado.Apellido
```

#### Botón teléfono

```
empleadoSeleccionado.telefono
```

#### Botón fecha de alta

```
empleadoSeleccionado.FechaIngreso
```

#### Botón "En plantilla"

```
empleadoSeleccionado.EnPlantilla
```

#### Botón de email
```
✉empleadoSeleccionado.email
```

#### Botón volver

```
Back()
```

## Modificación de empleado

En esta pantalla, podremos modificar los datos de cualquier empleado de la empresa, simplemente seleccionandolo.
También está directamente conectada con la pantalla del listado de los empleados, por medio del botón correspondiente.

El código del botón de editar es el siguiente:

```
UpdateContext({ editMode: true; selectedRecord: Table5.Selected })
```

El resultado de la página es el siguiente.

![image](https://github.com/user-attachments/assets/9cd07509-d938-44e1-809a-4fcb963b1751)

## Calendario de eventos

