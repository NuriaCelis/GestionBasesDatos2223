# UNIDAD 5. EDICIÓN DE LOS DATOS

## 1.- INSERCIÓN DE FILAS. INSTRUCCIÓN INSERT.

Para insertar filas en una tabla se utiliza la instrucción **INSERT**.

Mediante esta instrucción se pueden insertar una o varias filas con los valores que se especifiquen en la instrucción. 

```sql
INSERT …..   VALUES
INSERT …..   SET
```

Se pueden insertar los datos correspondientes a varias filas resultado de una consulta SELECT sobre otra u otras tablas.

```sql
INSERT …..  SELECT 
```

La sintaxis completa de **INSERT ….. VALUES** es la siguiente:

```sql
INSERT   INTO tabla  (col1, col2, ...)
    VALUES ({expr1 | DEFAULT} , {expr2 | DEFAULT}, ...),
	 ({expr1 | DEFAULT} , {expr2 | DEFAULT}, ...),
	……….
    [ ON DUPLICATE KEY UPDATE col_name1=expr[, col_name2=expr] ... ]
```

- Entre paréntesis se escriben los nombres de columnas en las que se asignan valores.
- Tras VALUES y entre paréntesis se especifican los valores que se asignan a las columnas para cada fila insertada.
- En una misma INSERT se pueden añadir o insertar varias filas.
- Se usa DEFAULT para indicar que se asigne a la columna el valor por defecto, si es que esa columna se ha diseñado con valor por defecto.
- Las columnas a las que no se asignan valores en INSERT, las que no están en la lista de columnas, reciben el valor por defecto o nulo. Si  no admiten nulos, la instrucción da error.
- La cláusula ON DUPLICATE KEY UPDATE establece que si al insertar una fila se produce un error por PRIMARY KEY duplicada o repetida, se asignen los valores que se indican tras esa cláusula.

**Ejemplo 1:** Insertar una fila en la tabla automóviles para un Seat Leon 2.0 TDI negro, matrícula 4751JVW, con extras GPS y SN y 20 kilómetros recorridos y estado no alquilado. No se carga el precio (o se carga el valor por defecto).

```sql
INSERT INTO automoviles (matricula, marca, modelo, color, kilometros, extras, alquilado) 
VALUES ('4751JVW', 'Seat', 'Leon 2.0 TDI', 'Negro', 20, 'GPS,SN', false);
```

Otra posible solución puede darse no especificando la lista de columnas, en cuyo caso, hay que indicar los valores para todas las columnas y en el orden en que están declaradas en la tabla.

```sql
INSERT INTO automoviles VALUES ('4751JVW', 'Seat', 'Leon 2.0 TDI', 'Negro', null, 20, 'GPS,SN', false);
```

**Ejemplo 2:** Insertar un nuevo contrato iniciado el 19 de febrero de 2018 por el cliente de DNI 00371569B del automóvil de matrícula 5678JRZ, con kilómetros iniciales 7659.

```sql
INSERT INTO  contratos (matricula, dnicliente, fini,kini) 
VALUES ('5678JRZ','00371569B ','2018-02-19',7659);
```

**Ejemplo 3:** Insertar un nuevo cliente de nombre Javier, apellidos: Quesada Gómez, ciudad:Madrid, direccion: C/ Marques de Otaiza 3, 4º B, tipo de permiso B.
 
```sql
INSERT INTO clientes (nombre, apellidos, localidad, direccion, carnet) 
VALUES ("Javier","Quesada Gómez","Madrid","C/ Marques de Otaiza 3, 4º B", "B");
```
¿Qué ocurre con esta inserción?

**Ejemplo 4:** Se tiene anotado un contrato que no se sabe si se insertó en la tabla de datos. Se sabe que ese contrato era el número 20 para la matrícula 2123JTB y el cliente con DNI 03549358G. La fecha de inicio debía ser 9 de enero de 2018 y la fecha de finalización el 21 de enero, los kilómetros iniciales eran 34323 y los finales 36545.

Insertar el nuevo contrato:

```sql
INSERT INTO contratos (numcontrato, dnicliente, matricula, fini, ffin, kini, kfin)
VALUES (20, '03549358G','2123JTB',  '2018-01-9', '2018-01-21', 34323, 36545);
```

Esta instrucción nos da error ya que existe el contrato número 20. Vamos a modificarla para que en caso de DUPLICATE KEY se asignen en el contrato existente los valores que se han indicado para la fecha final y kilómetros finales:

```sql
INSERT INTO contratos (numcontrato,dnicliente,matricula,fini,ffin,kini,kfin) VALUES (20, '03549358G','2123JTB',  '2018-01-9', '2018-01-21', 34323, 36545) 
ON DUPLICATE KEY UPDATE ffin='2018-01-21', kfin=36545;
```

**Ejemplo 5:** Se trata de insertar un contrato para un cliente con DNI 13987654C. El cliente quiere contratar en la fecha de hoy el automóvil de matrícula 4387JDD que tiene actualmente 23057 kilómetros.

```sql
INSERT INTO contratos (dnicliente,matricula,fini,kini) 
VALUES ('13987654C','4387JDD', curdate(), 23057);
```

Verás que se ha producido un error:

````sql
Error Code: 1452. Cannot add or update a child row: a foreign key constraint fails (`alquileres`.`contratos`, CONSTRAINT `fk_contrato_cliente` FOREIGN KEY (`dnicliente`) REFERENCES `clientes` (`dni`) ON DELETE NO ACTION ON UPDATE CASCADE)
```

El error se ha producido porque no se puede añadir una nueva fila en la que falla una restricción de clave ajena, la correspondiente al dni del cliente, ya que ese dni 13987654C no existe en la tabla clientes.

**Ejemplo 6:** 
Inserción de múltiples filas:

Insertar con una sola instrucción INSERT los clientes cuyos datos que se muestran a continuación y con el tipo de carnet B:


AQUI VA Imagen1.png



INSERT INTO clientes(dni,nombre, apellidos, direccion, localidad,fnac, fcarnet, carnet)
VALUES
('96401636R','Manuel','Gutierrez Motos',"Calle Barrio Camino",'Almansa','1992-02-19','2010-08-25','B’),
('1057451R','Pedro','Salas  Nieto',"Calle Camarreal",'Zaragoza','1970-12-07','1990-06-13','B’),
('66082349R','Alba','Casaus Rodriguez',"Bajada de San Juan",'Móstoles','1997-02-08','2015-02-21','B');
Sintaxis de INSERT …..  SET:


INSERT INTO table
SET col1={expr1 | DEFAULT}, col2={expr2 | DEFAULT}, ...
    [ ON DUPLICATE KEY UPDATE col_name1=expr1 , col_name2=expr2, ... ]

Se puede usar para los mismos casos que VALUES aunque nunca se puede usar para insertar múltiples filas.
Tras SET se asignan valores a cada una de las columnas de la fila a insertar, aunque no es necesario asignárselos a todas las columnas (solo a las que no admiten nulos y no tienen valor por defecto).
Se usa DEFAULT para indicar que se asigne a la columna el valor por defecto, si es que esa columna se ha diseñado con valor por defecto.
Las columnas a las que no se asignan valores en INSERT, las que no están en la lista de columnas, reciben el valor por defecto o nulo. Si  no admiten nulos, la instrucción da error.
La cláusula ON DUPLICATE KEY UPDATE establece que si al insertar una fila se produce un error por PRIMARY KEY duplicada o repetida, se asignen los valores que se indican tras esa cláusula.
Ejemplos de uso de INSERT ….. SET:

Ejemplo 7:

Insertar una fila en la tabla automóviles para un Seat Leon 2.0 TDI negro, matrícula 4751JVW, con extras GPS y SN y 20 kilómetros recorridos y estado no alquilado. No se carga el precio (o se carga el valor por defecto).

INSERT INTO automoviles SET 
matricula='4751JVW’, 
marca='Seat’, 
modelo= 'Leon 2.0 TDI’, 
color='Negro’, 
kilometros= 20, 
extras= 'GPS,SN’, 
alquilado= false;Sintaxis de INSERT …..  SELECT:


INSERT INTO tabla (col1,_col2, ...)
SELECT ...
    [ ON DUPLICATE KEY UPDATE col_name1=expr1, col_name2=expr2, ... ]

Permite insertar en TABLA tantas filas como filas resulten de ejecutar la SELECT que se aplica a la inserción.
Las columnas obtenidas en SELECT deben ser del mismo tipo o compatibles(no se necesita que tengan el mismo nombre) que las columnas correspondientes donde se van a insertar los datos.
Las columnas a las que no se asignan valores en INSERT, las que no están en la lista de columnas, reciben el valor por defecto o nulo. Si  no admiten nulos, la instrucción da error.
La cláusula ON DUPLICATE KEY UPDATE establece que si al insertar una fila se produce un error por PRIMARY KEY duplicada o repetida, se asignen los valores que se indican tras esa cláusula.
Ejemplos de uso de INSERT ….. SELECT:

Ejemplo 8:

Añadir los contratos de la tabla contratos2 a la tabla contratos.

INSERT INTO contratos SELECT * FROM contratos2;

Si ejecutamos actualmente esto en nuestra base de datos alquileres, se produce error ya que, al menos hay un contrato en contratos2 con número 1 y ese número de contrato también existe en contratos. Con que haya un error se rechazan todas las filas a insertar.

Error Code: 1062. Duplicate entry '1' for key 'PRIMARY'
Ejemplos de uso de INSERT ….. SELECT:

Ejemplo 8:

Siguiendo con el ejemplo anterior, para solucionar el error, vamos a hacer que el número de contrato que se inserte sea la suma del número de contrato en contratos 2 más el mayor número de contrato existente en la tabla contratos (supongamos que es el 24).

INSERT INTO contratos(numcontrato, matricula,dnicliente, fini, ffin, kini, kfin) SELECT numcontrato+24, matricula,dnicliente, fini, ffin, kini, kfin FROM contratos2;

INSERT INTO contratos(numcontrato, matricula,dnicliente, fini, ffin, kini, kfin)SELECT numcontrato+(select max(numcontrato) from contratos), matricula,dnicliente, fini, ffin, kini, kfin FROMcontratos2;
Ejemplos de uso de INSERT ….. SELECT:

Ejemplo 9:

El cliente con dni 08785691K ha comunicado que hoy mismo quiere alquilar todos los automóviles de la marca Seat que no estén alquilados actualmente. 
Insertar los nuevos contratos para ese cliente con la fecha actual y las matriculas de esos automóviles. 
Se insertará en kilómetros iniciales los kilómetros registrados en la tabla automóviles y se comprobará que no estén alquilados.


INSERT INTO contratos(matricula,dnicliente, fini, kini) 
SELECT matricula, '08785691K',curdate(),kilometros FROM automoviles WHERE alquilado=false AND marca='seat';
Ejemplos de uso de INSERT ….. SELECT:

Ejemplo 10:

Mariano Dorado ha comunicado que hoy mismo quiere alquilar todos los automóviles de precio inferior a 70€ que no estén alquilados actualmente. Insertar los nuevos contratos para ese cliente suponiendo que no podemos conocer su dni primero para después usarlo en INSERT.


INSERT INTO contratos (matricula,dnicliente, fini, kini) 
SELECT matricula, dni,curdate(),kilometros FROM automoviles,clientes WHERE alquilado=false AND precio<70 AND nombre='mariano' AND apellidos='dorado’;

ES UNA CONSULTA CON PRODUCTO CARTESIANO, HACE TODAS LAS COMBINACIONES DE LOS COCHES CON EL CLIENTE.
Ejemplos de uso de INSERT ….. SELECT:

Ejemplo 11:

Supongamos que en la base de datos LIGATERCERA tenemos una tabla CALENDARIO(eqLocal, eqVisitante, numjornada, numpartido,fecha) donde la clave principal está formada por las dos primeras columnas (los códigos del equipo local y del visitante) y las otras columnas admiten nulos, insertar todos los posibles enfrentamientos o partidos entre los equipos de la liga.


INSERT INTO calendario  (eqLocal, eqVisitante) 
SELECT a.codeq, b.codeq FROM equipos AS a, equipos AS b WHERE a.codeq!=b.codeq;


hoja de ejercicio 6.01 y 6.01 bis