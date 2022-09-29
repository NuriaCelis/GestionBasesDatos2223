# UNIDAD 3. DISEÑO FÍSICO DE LA BASE DE DATOS.

## 1.- CARACTERÍSTICAS DEL DISEÑO FÍSICO

El diseño físico se realiza a partir del diseño lógico (grafo relacional). Consta de todas las instrucciones SQL necesarias para implementar la base de datos en el DBMS.

1. Hay que crear las tablas eligiendo un nombre adecuado. Se establecen los tipos de tabla y restricciones de tabla. Para cada tabla se definen las columnas, sus nombres y los tipos de datos que contienen.
2. Se establecen las restricciones necesarias sobre las columnas de las tablas (PRIMARY KEY, NOT NULL, UNIQUE, FOREIGN KEY, etc.)
3. Se crean vistas. (Tablas virtuales que permiten simplificar búsquedas complejas)
4. Se crean procedimientos (conjuntos de queries), funciones y triggers (objetos que se activan cuando sucede una acción).
5. Se establecen propiedades sobre las tablas (motor de almacenamiento, carpeta de almacenamiento, valor autoincrement, particiones, etc.)

## 2.- HERREMIENTAS GRÁFICAS PARA LA IMPLEMENTACIÓN DE LA BASE DE DATOS.


Vemos algunas de las herramientas gráficas gratuitas que podemos encontrar:

MySQL Workbench

![MySqlWorkbech](img/imagen1.png)

phpMyAdmin (Requiere servicio Apache con motor PHP)

![phpMyAdmin](img/imagen2.png)

HeidiSQL

![HeidiSQL](img/imagen3.png)

**Realiza el siguiente ejercicio:**

1. Crea una base de datos EmpTransportes.

    - ¿Cuál es la instrucción SQL para crear la base de datos?
    - Selecciona la nueva base de datos e identifica los botones de la barra de herramientas para añadir tablas, vistas y rutinas.

2. Crea gráficamente en la base de datos EmpTransportes, la tabla camiones y __copia la instrucción__ SQL de creación de la tabla.

![grafico](img/imagen4.png)

3.  Abre una ventana para editar y ejecutar instrucciones SQL. Edita la instrucción copiada para crear la tabla camioneros.

# 3.- EL LENGUAJE DE DEFINICIÓN DE DATOS (DDL)

Desde este momento comenzamos a usar el lenguaje SQL (Structured Query Language).Se trata de un lenguaje estandarizado para interactuar mediante consultas sobre sistemas de bases de datos relacionales. Se entiende por consulta cualquier petición que se hace al SGBD.

Los SGBD relacionales incluyen siempre alguna herramienta para ejecutar instrucciones SQL.Desde el primer estándar ANSI-SQL de 1986, se han ido desarrollando varios estándares o versiones del SQL. El último es SQL-2016.

Un estándar establece reglas de sintaxis y funcionamiento del repertorio de instrucciones SQL. SQL consta de un repertorio de instrucciones. En general, los SGBD incluyen prácticamente todo el repertorio de instrucciones del estándar SQL y con la misma sintaxis. Pero puede ocurrir que:

- No incluyan alguna instrucción.
- Incluyan alguna instrucción propia no perteneciente al estándar.
- En algunas instrucciones la sintaxis pueda variar ligeramente por no incluir alguna funcionalidad o por incluir alguna funcionalidad propia.

### Interpretación de la sintaxis de una instrucción SQL.

Cuando nos dan la sintaxis completa de una instrucción SQL, por ejemplo, en la documentación oficial de MySQL, tenemos algo como esto:

```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [create_specification]  

Create_specification: [DEFAULT] CHARACTER SET [=] charset_name | [DEFAULT] COLLATE [=] collation_name
```

Esto tenemos que saber interpretarlo para construir correctamente las instrucciones. Hay que tener en cuenta:

- Las palabras en mayúsculas son palabras reservadas SQL.

- Las palabras en minúsculas y cursiva son parámetros sustituibles y cuyo valor decide el usuario. Así, por ejemplo db_name indica que tenemos que escribir el nombre de la base de datos.

- Algo entre corchetes indica que es opcional, es decir, que podemos escribir la instrucción sin esa parte. Si se escribe lo opcional, no hay que escribir los corchetes.

- Algo entre llaves indica que tenemos que elegir entre uno de los elementos (separados por |) que hay dentro de las llaves.

- Puntos suspensivos indica que podemos introducir una lista de valores de lo anterior que hay.

### Subconjuntos del lenguaje SQL

En función del tipo de operaciones realizadas, el conjunto de instrucciones SQL se puede considerar dividido en tres subconjuntos de instrucciones.

1. DDL (Data Definition Language): Son todas las instrucciones que permiten establecer las estructura de los datos en las bases de datos. En definitiva, son las instrucciones para realizar el diseño físico de las bases de datos.
2. DML (Data Manipulation Language): Son las instrucciones que sirven para manipular los datos que se almacenan en las bases de datos (consultar, insertar, modificar, eliminar).
3. DCL (Data Control Language): Son las instrucciones de control de acceso a los datos (gestionar usuarios y privilegios, realizar transacciones, bloquear, etc.).

Las principales instrucciones del lenguaje DDL son las siguientes:

- CREATE DATABASE
- CREATE TABLE
- CREATE INDEX
- CREATE VIEW
- CREATE PROCEDURE
- CREATE FUNCTION
- CREATE TRIGGER
- ALTER (DATABASE, TABLE, VIEW, …)
- DROP (DATABASE, TABLE, VIEW, ….)

La sintaxis de la instrucción para **crear una base de datos** es la siguiente:

```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [create_specification]  

Create_specification: [DEFAULT] CHARACTER SET [=] charset_name | [DEFAULT] COLLATE [=] collation_name
```

En MySql es lo mismo usar DATABASE o SCHEMA.

La cláusula IF NOT EXISTS hace que no se intente crear la base de datos si es que existe. Así  no se produce un error de ejecución.

CHARACTER SET permite especificar el conjunto de caracteres o, lo que es lo mismo, como se codifican internamente los caracteres (utf8, latin1, etc.)

COLLATE establece los criterios para ordenar y comparar datos alfabéticamente (por ejemplo, spanish_ci).

**Realiza el siguiente ejercicio:**

1. Crea la base de datos EmpTransportes.

Solución: 

```sql
    CREATE DATABASE EmpTransportes;
```

Si queremos que la instrucción  no de error en caso de existir EmpTransportes:

```sql
    CREATE DATABASE IF NOT EXISTS EmpTransportes;
```

Si queremos que la base de datos se cree para usar el conjunto de caracteres latin1 (en lugar de utf8 usado por defecto) y con ordenación alfabética para el español (por defecto, se usa general_ci):

```sql
    CREATE DATABASE IF NOT EXISTS EmpTransportes CHARSET latin1 COLLATE latin1_spanish_ci;
```

La instrucción para **mostrar las bases de datos** montadas en el servidor es la siguiente:

```sql
    SHOW DATABASES;
```

La sintáxis de la instrucción para **modificar una base de datos** es la siguiente:

```sql
    ALTER {DATABASE | SCHEMA} [db_name] alter_specification;
    alter_specification: [DEFAULT] CHARACTER SET [=] charset_name | [DEFAULT] COLLATE [=] collation_name 
```

 La síntáxis de la instrucción para **eliminar una base de datos** es la siguiente:

```sql
     DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;
```

 Para que podamos ejecutar instrucciones sobre una base de datos existente, es necesario tenerla en uso o abrirla:

```sql
	 USE  db_name;
```
**Realiza el siguiente ejercicio:**

1. Probamos las siguientes instrucciones en MySql Command Line Client:
    - Entramos con la contraseña de root
    - Mostramos las bases de datos existentes:
```sql
        Show databases;
```
   - Utilizamos la BD emptransportes:
```sql
        Use emptransportes;
```
   - Mostramos las tablas de dicha base de datos:
```sql
        Show tables;
```

Tipos de datos:

Vamos a ver de forma resumida los tipos de datos que podemos usar en MySQL para las columnas de las tabla. Estos se pueden clasificar en:

Numéricos
Cadenas de caracteres
Cadenas de bytes o binarias
Fecha y hora
Booleanos
Enumerados
Conjuntos

Tipos de datos numéricos:
(aqui va una tabla)




