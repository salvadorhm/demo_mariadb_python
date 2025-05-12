# Demo de bases de datos en codespace y gitpod

Uso de python3 con las siguientes funciones: insertar, borrar, actualizar, buscar y listar registros de una tabla.

## 1. Configurar codespace

1.1 Actualizar versiones de librerias y paquetes instalables

````shell
$ sudo apt-get update
````

1.2 Verificar el estado de los servicios

````shell
sudo service --status-all

ó

sudo service mysql status
````

1.3 Instalar MariaDB

Para este proyecto se utilizara MariaDB como servidor de base de datos.

````shell
sudo apt-get install mariadb-server -y
````

1.4 Detener el servidor

En especifico con codespaces de GitHub es necesario detener el servicio de mysql para configurarlo y poder acceder a el.

````shell
sudo /etc/init.d/mysql stop

ó

sudo service mysql stop
````

1.5 Iniciar el servidor

Hay que reiniciar el servicio de mysql en modo seguro para poder iniciar sesión con el usuario root (default)

````shell
sudo mysqld_safe --skip-grant-tables & service mysql start
````

1.6 Conectando con el servidor MySQL

Iniciar una sesión dentro de mysql

````shell
mysql -u root
````

**NOTA**: En este momento el servidor esta utilizando la configuración predeterminada, lo que lo hace poco seguro.

Una vez que se verifico que se puede establecer comunicación con el servidor se hará una configuración segura, para eso hay que salir de **MariaDB** y realizar la configuración.

Para salir del shell de **MariaDB** se utiliza exit

````shell
MariaDB [(none)]> exit;
````

1.7 Configuración segura del servidor de base de datos

El servidor de base de datos en este momento no tiene contraseña para el usuario root, por lo que desde el shell se ejecuta el siguiente comando.

````shell
mysql_secure_installation
````

1.8 Conecarse al servidor después de configurarlo

Iniciar una sesión dentro de mysql utilizando la nueva contraseña que se establecio.

NOTA: Es importante utilizar sudo para la conexion.


````shell
sudo mysql -u root -p
````

## 2. Script para crear la base de datos

Para este ejercicio se tendrá una base de datos con una sola tabla.

2.1 Diccionario de datos de la tabla **personas**.

La descripción de los campos de la tabla **personas** permite tener el contexto de lo que se va a realizar.

|Atributos|Campo|Tipo de dato|Descripción|
| -- | -- | -- | -- |
| PK | id_persona | int | Identificador de la persona |
| - | nombre | varchar(50) | Nombre de la persona |
| - | primer_apellido | varchar(50) | Primer apellido de la persona |
| - | segundo_apellido | varchar (50) | Segundo apellido de la persona |
| - | email | varchar(100) |  Email de la persona |
| - | telefono | varchar(10) | Teléfono de la persona |

2.2 Script para crear la base de datos

* Crear la base de datos **db_agenda**.
* Crear la tabla **personas**.
* Insertar 2 registros en la tabla **personas**.


**NOTA**: el script crea un nuevo usuario de nombre **user** con el passoword **1234**, y le asigna todos los privilegios sobre la base de datos **db_agenda**, este será el usuario que estar utilizado para trabajar con la base de datos.

+ **host**: localhost
+ **user**: user
+ **password**: 1234
+ **port**: 3306
+ **dbname**: db_agenda

````sql
CREATE DATABASE db_agenda;

USE  db_agenda;

CREATE TABLE personas(
    id_persona int AUTO_INCREMENT PRIMARY KEY,
    nombre varchar(50) NOT NULL,
    primer_apellido varchar(50) NOT NULL,
    segundo_apellido varchar(50) NOT NULL,
    email varchar(100) NOT NULL,
    telefono varchar(10) NOT NULL
);

INSERT INTO personas (nombre,primer_apellido,segundo_apellido,email,telefono)
VALUES 
("Dejah", "Thoris", "Barsoon","dejah@email.com","1234567890"),
("John", "Carter", "Earth", "john@email.com","2345678901");

SELECT * FROM personas;

CREATE USER 'user'@'localhost' IDENTIFIED BY '1234';
GRANT ALL PRIVILEGES ON db_agenda.* TO 'user'@'localhost';
FLUSH PRIVILEGES;

SELECT "Usuario creado";
````

2.3 Crear la base de datos desde MariaDB shell

````shell
MariaDB [(none)]> source db_agenda.sql
````

2.4 Salir de la MariaDB

Para salir del shell de **MariaDB** se utiliza exit

````shell
MariaDB [(none)]> exit;
````

## 3. Ambiente virtual

3.1 Crear el ambiente virtual

El ambiente virutal permite tener un espacio aislado donde se instalarán unicamente las librias necesarias, lo que permite evitar conflictos de versiones de librerias con otros proyectos.

````shell
$  python3 -m venv .venv
````

3.2 Iniciar el ambiente virtual

Una vez creado  el ambiente virtual se debe inicializar con el siguiente comando.

````shell
$ source .venv/bin/activate
````
Para saber si ya se activo el ambiente virtual debe aparacer (venv) en el shell, como se muestra en el siguiente ejemplo.

````shell
(.venv)$
````
3.3 Actualizar PIP

Antes de instalar liberias es recomendable actualizar pip, esto permite que se descarguen las últimas versiones de las librerías.

````shell
(.venv)$ pip install --upgrade pip
````

3.4 Instalar las librerias


3.6 Salir del ambiente virtual

Cuando ya no se requiera utilzar el ambiente virtual se puede salir con el siguiente comando, y cuando se requiera utilizarlo nuevamente se usa el comando del paso 3.2

````shell
deactivate
````
