<!-- language-all: lang-sql -->
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

# 4.- CREACIÓN, MODIFICACIÓN Y ELIMINACIÓN DE BASES DE DATOS

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
    SHOW databases;
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
    SHOW databases;
```
   - Utilizamos la BD emptransportes:
```sql
    USE emptransportes;
```
   - Mostramos las tablas de dicha base de datos:
```sql
    SHOW tables;
```

# 5.- TIPOS DE DATOS. VALORES Y OPERADORES

Vamos a ver de forma resumida los tipos de datos que podemos usar en MySQL para las columnas de las tabla. Estos se pueden clasificar en:

- Numéricos
- Cadenas de caracteres
- Cadenas de bytes o binarias
- Fecha y hora
- Booleanos
- Enumerados
- Conjuntos

### Tipos de datos numéricos

| Tipo de dato | Rango de representación |
| ------------- | ------------- |
| TINYINT  | Enteros entre -128 y +127. Sin signo entre 0 y 255. Ocupan 1 byte  | 
| SMALLINT  | Enteros entre -32768 y +32767. Sin signo entre 0 y 65535. Ocupan 2 bytes  | 
| MEDIUMINT  | Enteros entre aproximadamente -8 millones y + 8 millones. Sin signo aproximadamente entre 0 y 16 millones. Ocupan 3 bytes.  | 
| INT, INTEGER | Enteros entre aproximadamente -2 mil millones y +2 mil millones. Sin signo aproximadamente entre 0 y 4 mil millones. Ocupan 4 bytes. | 
| BIGINT  | Enteros entre aproximadamente -1019 y  +1019. Sin signo aproximadamente entre 0 y 2x1019 |
| FLOAT  | Reales en coma flotante de precisión simple (6dígitos). Admite negativos entre -3.4x1038 y  -1.2x10-38  , 0  y positivos entre 1.2x10-38 y  3.4x10+38 |
| DOUBLE, REAL | Reales en coma flotante de precisión doble (12dígitos).Permite números negativos entre -1.8x10308 y  -2.2x10-308  , 0 y positivos entre 2.2x10-308 y  1.8x10+308 |
| DECIMAL, NUMERIC | Número en coma fija (con una posición fija de la coma decimal. Por defecto sirve para números de hasta 10 cifras en la parte entera sin cifras decimales. La variante DECIMAL(M,D) permite especificar en M el número total de dígitos y en D el número de decimales |

Todos los enteros se pueden definir en la forma TIPO(N) donde N indicará el número de cifras con que se presenta o edita el número.

Todos los reales se pueden definir en la forma TIPO(N,D) donde N indicará el número total de cifras con que se presenta o edita el número (de 0 a 24) y D es el número de decimales.

Todos los tipos numéricos admiten los modificadores UNSIGNED y ZEROFILL. UNSIGNED especifica que el entero es sin signo y ZEROFILL que un número que ocupa N cifras se muestra en pantalla rellenando con ceros las cifras no significativas del número. 

Ejemplos de definición de columnas:

```sql
numPrimitiva TINYINT UNSIGNED;
numLoteria INT(5) UNSIGNED ZEROFILL;
pesoAtomico DOUBLE;
tempMedia DECIMAL(4,2);
precioUnidad FLOAT;
```

### Tipos de datos cadena de caracteres

| Tipo de dato | Rango de representación |
| ------------- | ------------- |
| CHAR(N)  | Cadena de longitud fija de N caracteres. Cualquier valor que se almacene ocupará lo correspondiente a N caracteres. Si se cargan menos caracteres se rellena con espacios por la derecha. Admite hasta 255 caracteres | 
| VARCHAR(N)  | Cadena de longitud variable hasta un máximo de N caracteres. Si se carga una cadena con menos de N caracteres, ocupará tanto espacio como necesiten los caracteres cargados (no se rellena con espacios). Admite hasta 65535 caracteres. | 
| TINYTEXT  | Igual que VARCHAR para cadenas de hasta 255 caracteres. | 
| TEXT | En lo descrito es igual a VARCHAR. Tiene algunas pequeñas diferencias. En general es más conveniente usar VARCHAR por compatibilidad con otros SGBD. Hasta 65535 caracteres. | 
| MEDIUMTEXT  | Igual que VARCHAR para cadenas de hasta 16 millones de caracteres. |
| LONGTEXT  | Igual que VARCHAR para cadenas de hasta 4 mil millones de caracteres. |

Ejemplos de definición de columnas de tipos cadenas de carcateres:

```sql
nombreCiclo VARCHAR(80),
dniProfesor CHAR(9);
codPostal CHAR(5);
signoQuiniela CHAR;  -- Equivalente a usar CHAR(1)
codPais CHAR(2);
argPelicula TEXT(500);   -- Se puede y es más recomendable usar VARCHAR(500)
```

### Tipos de datos cadena de bytes o binarias

Permiten almacenar secuencias de bytes, por ejemplo el contenido de ficheros. También permiten almacenar cadenas de texto, en cuyo caso, al comparar se diferencia entre mayúsculas y minúsculas. No es adecuado definir una columna para cargar en ella el contenido de un fichero. Para ese caso es mejor definirla para que contenga un texto con el nombre y ubicación del fichero en el disco.

| Tipo de dato | Rango de representación |
| ------------- | ------------- |
| BYNARY(N)  | Cadena de longitud fija de N bytes. Cualquier valor que se almacene ocupará lo correspondiente a N bytes. Si se cargan menos caracteres se rellena con espacios por la derecha. Admite hasta 255 caracteres | 
| VARBINARY(N)  | Similar a VARCHAR para cadenas binarias. | 
| TINYBLOB(N)  | Similar a TINYTEXT para cadenas binarias. | 
| BLOB(N) | Similar a TEXT para cadenas binarias. | 
| MEDIUMBLOB(N)  | Similar a MEDIUMTEXT para cadenas binarias. |
| LONGBLOB(N)  | Similar a LONGTEXT para cadena binarias. |

### Tipos de datos para fecha y hora

| Tipo de dato | Rango de representación |
| ------------- | ------------- |
| DATE  | Permite almacenar fechas en el formato ‘aaaa-mm-dd’. Se pueden usar otros separadores. El rango soportado es desde 1000-1-1 hasta 9999-12-31 | 
| TIME | Permite almacenar datos de tipo hora con el formato ‘hh:mm:ss’. Se pueden usar otros separadores. El rango soportado es desde -838:59:59 hasta +838:59:59. | 
| DATETIME | Permite almacenar datos con fecha y hora con el formato:
‘aaaa-mm-dd  hh:mm:ss’ | 
| TIMESTAMP | Permite almacenar datos con fecha y hora con el formato:
‘aaa-mm-dd  hh:mm:ss’
El rango de representación es entre 1970-01-01  00:00:00 y 2037-12-31   23:59:59. Es útil para registrar cuando se producen operaciones de inserción y modificación sobre columnas de este tipo. Reciben por defecto la fecha y hora actuales cuando no se carga en ellas ningún valor. | 

### Tipos de datos booleanos

En MySQL se tiene el tipo BOOLEAN para representar valores booleanos (veradadero o falso). 
La realidad es que el dato que se almacena en un BOOLEAN es un tipo TINYINT(1). El valor 0 almacenado representa false y el valor 1 representa true. 

Para hacer referencia a los valores que tiene un BOOLEAN podemos usar indistintamente 0 o false y 1 o true, aunque es mejor usar false y true.

### Tipos de datos enumerados

Es un tipo de dato que puede contener uno de entre un conjunto de textos definidos en la declaración del dato. Un dato de tipo enumerado se define como ENUM(‘cad1’, ‘cad2’, ……, ‘cadN’)

Para definir una columna dia para contener los días de la semana, haremos:

```sql
	Dia ENUM(‘lunes’,’martes’,’miercoles’,’jueves’,’viernes’,’sabado’,’domingo’)
```

Realmente en una columna ENUM se almacenan los valores índice del dato guardado comprendidos entre 1 y el número de elementos de la enumeración. Un dato ENUM se puede manejar indistintamente con los valores definidos en la enumeración o con los índices.
Los datos enumerados se ordenan por el índice.

### Tipos de datos conjuntos

Es un tipo de datos que puede contener varios valores o ninguno de entre un conjunto de textos definidos en la declaración del dato. Un dato de tipo conjunto se define como SET(‘cad1’, ‘cad2’, ……, ‘cadN’).

Para definir una columna Formato para contener el formato de letra fuente haremos:

```sql
	formato SET(‘negrilla’,’subrayado’,’cursiva’)
```

Al insertar valores en una columna del tipo anterior podemos insertar:

```sql
		‘negrilla’
		‘cursiva’
		‘negrilla,cursiva’
```

Si se insertan dos o más valores del conjunto, los valores se han de escribir respetando el orden en que se definieron en el conjunto. Sería inválida la inserción de ‘cursiva,negrilla’.
Los valores no válidos que se traten de insertar se ignoran.

Para comprobar si un dato SET contiene un determinado grupo de valores se usa la función FIND_IN_SET. También se puede usar el operador LIKE adecuadamente.

### Representación de valores literales

Las cadenas de caracteres se representan entre comillas dobles o entre comillas simples. Para representar comillas dentro de un literal cadena de caracteres se tienen que preceder de \. Los caracteres especiales también se tienen que preceder de \.

En los datos numéricos se representa como separador de parte entera y decimal el carácter punto. Los valores correspondientes a numéricos como flotante se pueden representar en notación exponencial (por ejemplo, 2.7562e+12). Y se pueden representar valores hexadecimales (precedidos de 0x, por ejemplo, 0x3A24FF).

Los datos booleanos se representan con true o false.

Los valores nulos (sin valor asignado) se representan con NULL.

### Operadores

En las instrucciones SQL podemos usar un amplio número de operadores. De ellos, los más importantes son:

Operadores de comparación y pertenencia

De igualdad, desigualdad:    =	!=
Mayor que, mayor o igual que:   >		>=
Menor que, menor o igual que:  <		<=
Es nulo, no es nulo:		IS NULL		IS NOT NULL
Pertenencia a un rango:    BETWEEN 1 AND 100
Pertenencia a un conjunto:	 IN(1,2,4,8)



Operadores lógicos
Y Lógico:  AND
```sql
	nota >=5 AND nota <=10
```
O lógico: OR
```sql
	nota>10 OR nota <0
```
Negación: NOT
```sql
	NOT(x>=5)
```

# 6.- ADMINISTRACIÓN DE TABLAS

Las instrucciones DDL para administrar tablas permiten:

- Crear un tabla con un nombre, definiendo, además, las columnas que contiene, sus tipos y las restricciones.
- Establecer propiedades de una tabla.
- Modificar la estructura de una tabla (añadir una columna, eliminar una columna, modificar las columnas PRIMARY KEY, añadir una FOREIGN  KEY, etc.)
- Eliminar una tabla.
- Crear un índice.
- Eliminar un índice.
- Renombrar una tabla.

## 6.2.- Sintaxis de la instrucción CREATE TABLE

La instrucción SQL para crear una tabla es CREATE TABLE. La sintaxis completa de esta instrucción es bastante compleja. La puedes ver dentro de la documentación oficial de MySQL.

https://dev.mysql.com/doc/refman/8.0/en/create-table.html

Vamos a explicar la sintaxis de una forma más simple:

```sql
CREATE  [TEMPORARY] TABLE  [IF NOT EXISTS] nombre_tabla
(
	nombre_columna1   tipo   [restricciones_tipo_1],
	nombre_columna2	   tipo   [restricciones_tipo_1],
	………………………….
	[restricción_tipo_2	],
	[restricción_tipo_2	],
	……………………………………….
)  [opciones_tabla];
```

Interpretación de la sintaxis:

1. La cláusula TEMPORARY hace que la tabla creada es temporal para la sesión cliente en ejecución. Al salir de la sesión la tabla se elimina automáticamente.

2. La cláusula IF NOT EXISTS hace que el servidor no devuelva un error cuando se intenta crear una tabla y ya existe.

3. Se abre y cierra un paréntesis. Entre el paréntesis se escriben separadas por comas todas las definiciones de columnas. Después de las definiciones de las columnas se escriben separadas por comas, si las hay,  las definiciones de restricciones en la tabla.
4. Al finalizar la definición de columnas se escriben todas las opciones o propiedades de la tabla. Si no se escribe ninguna se establecen las propiedades por defecto.

5. En la definición de cada columna se indica su nombre, su tipo (con los modificadores) y restricciones que se le aplican:
    - PRIMARY KEY      no es recomendable usarla en la definición de columnas
    - UNIQUE
    - NOT NULL
    - DEFAULT valor
    - AUTO_INCREMENT
    - GENERATED ALWAYS AS (expresión)



**Realiza el siguiente ejercicio:**

- Crear una tabla familiasprof que almacenará las familias profesionales de FP. La tabla tiene una columna código de la familia que se representa con tres letras y un nombre de la familia profesional. Esas columnas no admiten nulos.

- Crear la tabla familiasprof para que reciba en nomfamilia el valor “desconocida” cuando no se introduzca el nombre de una familia al insertar una fila.

[Solución](#Soluciones)

1. Restricciones tipo 2. Se pueden definir las siguientes restricciones que se aplican a una sola columna o a varias columnas de la tabla:

a.- [CONSTRAINT [simbolo]] PRIMARY KEY (columna1,...)

b.- INDEX [nom_indice] (columna 1,...)

c.- [CONSTRAINT [simbolo]] UNIQUE [nom_indice] (columna 1,...)

d.- FULLTEXT[nom_indice] (columna 1,...)

e.- [CONSTRAINT [simbolo]] FOREIGN KEY (colAjena1,...) REFERENCES tblReferenciada (colReferenciada 1,...)
[ON DELETE opcion_integridad][ON UPDATE opcion_integridad]

7. Restricciones de comportamiento de FOREIGN KEY para modificación (ON UPDATE).

- ON UPDATE RESTRICT : No se puede modificar un valor de la clave primaria en la tabla referenciada si se tienen filas con ese valor en la clave ajena de la tabla hija. Por ejemplo, si en una tabla paises se intenta modificar el identificador de España, no se podría hacer la modificación si en una tabla ciudades hubiera ciudades con ese código de país en su clave ajena.
- ON UPDATE NO ACTION : Comportamiento por defecto, idéntico a RESTRICT.
- ON UPDATE CASCADE : Lo más adecuado normalmente. Si se modifica el valor de la clave primaria en la tabla referenciada, se modifican los valores en la clave ajena de todas las filas de la tabla hija que tuvieran el valor modificado. Por ejemplo, si en la tabla paises se modifica el código de España, en todas las filas de la tabla de ciudades cuyo código de país fuera El de España, se modificaría ese valor.
- ON UPDATE SET NUL : Si se modifica el valor de la clave primaria en la tabla referenciada, se ponen a NULL o valor nulo los valores en la clave ajena de todas las filas de la tabla hija que tuvieran el valor modificado.

7. Restricciones de comportamiento de FOREIGN KEY para borrado (ON DELETE).

- ON DELETE RESTRICT : No se puede eliminar una fila en la tabla referenciada si se tienen filas relacionadas por clave ajena de la tabla hija. Por ejemplo, si en una tabla paises se intenta eliminar la fila de España, no se podría hacer la eliminación si en una tabla ciudades hubiera ciudades de españa (con la clave ajena de pais al valor de España).
- ON DELETE NO ACTION : Comportamiento por defecto, idéntico a RESTRICT.
- ON DELETE CASCADE : Es peligroso usarlo porque puede eliminarnos mucha información. Si se elimina una fila en la tabla referenciada, se eliminan todas las filas relacionadas en la tabla hija. Por ejemplo, si en la tabla paises se elimina la fila de España, se eliminarían en la tabla ciudades todas las ciudades de España.
- ON DELETE SET NUL : Si se elimina una fila en la tabla referenciada, se pone a nulo el valor de la clave ajena de todas las filas relacionadas en la tabla hija. Por ejemplo, si en la tabla paises se elimina la fila de España, se establecerá que el país de todas las ciudades que pertenecían a España es NULL.

<a name="Soluciones"></a>

- Crear una tabla familiasprof que almacenará las familias profesionales de FP. La tabla tiene una columna código de la familia que se representa con tres letras y un nombre de la familia profesional. Esas columnas no admiten nulos.

```sql
CREATE TABLE familiasprof (
   codfamilia CHAR(3) NOT NULL,
   nomfamilia VARCHAR(50) NOT NULL);
```
- Crear la tabla familiasprof para que reciba en nomfamilia el valor “desconocida” cuando no se introduzca el nombre de una familia al insertar una fila.

```sql
CREATE TABLE familiasprof (
   codfamilia CHAR(3) NOT NULL,
   nomfamilia VARCHAR(50) NOT NULL DEFAULT 'desconocida');
```