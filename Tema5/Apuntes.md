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

```sql
Error Code: 1452. Cannot add or update a child row: a foreign key constraint fails (`alquileres`.`contratos`, CONSTRAINT `fk_contrato_cliente` FOREIGN KEY (`dnicliente`) REFERENCES `clientes` (`dni`) ON DELETE NO ACTION ON UPDATE CASCADE)
```

El error se ha producido porque no se puede añadir una nueva fila en la que falla una restricción de clave ajena, la correspondiente al dni del cliente, ya que ese dni 13987654C no existe en la tabla clientes.

**Ejemplo 6:** Inserción de múltiples filas. Insertar con una sola instrucción INSERT los clientes cuyos datos que se muestran a continuación y con el tipo de carnet B:

```sql
INSERT INTO clientes(dni,nombre, apellidos, direccion, localidad,fnac, fcarnet, carnet)
VALUES
('96401636R','Manuel','Gutierrez Motos',"Calle Barrio Camino",'Almansa','1992-02-19','2010-08-25','B’),
('1057451R','Pedro','Salas  Nieto',"Calle Camarreal",'Zaragoza','1970-12-07','1990-06-13','B’),
('66082349R','Alba','Casaus Rodriguez',"Bajada de San Juan",'Móstoles','1997-02-08','2015-02-21','B');
```

![Insert](img/Imagen1.png)

La sintaxis de **INSERT …  SET** es la siguiente:

```sql
INSERT INTO table
SET col1={expr1 | DEFAULT}, col2={expr2 | DEFAULT}, ...
[ ON DUPLICATE KEY UPDATE col_name1=expr1 , col_name2=expr2, ... ]
```

- Se puede usar para los mismos casos que VALUES aunque nunca se puede usar para insertar múltiples filas.
- Tras SET se asignan valores a cada una de las columnas de la fila a insertar, aunque no es necesario asignárselos a todas las columnas (solo a las que no admiten nulos y no tienen valor por defecto).
- Se usa DEFAULT para indicar que se asigne a la columna el valor por defecto, si es que esa columna se ha diseñado con valor por defecto.
- Las columnas a las que no se asignan valores en INSERT, las que no están en la lista de columnas, reciben el valor por defecto o nulo. Si  no admiten nulos, la instrucción da error.
- La cláusula ON DUPLICATE KEY UPDATE establece que si al insertar una fila se produce un error por PRIMARY KEY duplicada o repetida, se asignen los valores que se indican tras esa cláusula.

**Ejemplo 7:**Insertar una fila en la tabla automóviles para un Seat Leon 2.0 TDI negro, matrícula 4751JVW, con extras GPS y SN y 20 kilómetros recorridos y estado no alquilado. No se carga el precio (o se carga el valor por defecto).

```sql
INSERT INTO automoviles SET 
matricula='4751JVW’, 
marca='Seat’, 
modelo= 'Leon 2.0 TDI’, 
color='Negro’, 
kilometros= 20, 
extras= 'GPS,SN’, 
alquilado= false;Sintaxis de INSERT …..  SELECT:
```
Tenemos otra sintaxis para **INSERT combinado con SELECT**:

```sql
INSERT INTO tabla (col1,_col2, ...)
SELECT ...
[ ON DUPLICATE KEY UPDATE col_name1=expr1, col_name2=expr2, ... ]
```

- Permite insertar en TABLA tantas filas como filas resulten de ejecutar la SELECT que se aplica a la inserción.
- Las columnas obtenidas en SELECT deben ser del mismo tipo o compatibles(no se necesita que tengan el mismo nombre) que las columnas correspondientes donde se van a insertar los datos.
- Las columnas a las que no se asignan valores en INSERT, las que no están en la lista de columnas, reciben el valor por defecto o nulo. Si  no admiten nulos, la instrucción da error.
- La cláusula ON DUPLICATE KEY UPDATE establece que si al insertar una fila se produce un error por PRIMARY KEY duplicada o repetida, se asignen los valores que se indican tras esa cláusula.

**Ejemplo 8:** Añadir los contratos de la tabla contratos2 a la tabla contratos.

```sql
INSERT INTO contratos SELECT * FROM contratos2;
```

Si ejecutamos actualmente esto en nuestra base de datos alquileres, se produce error ya que, al menos hay un contrato en contratos2 con número 1 y ese número de contrato también existe en contratos. Con que haya un error se rechazan todas las filas a insertar.

```sql
Error Code: 1062. Duplicate entry '1' for key 'PRIMARY'
```

Siguiendo con el ejemplo anterior, para solucionar el error, vamos a hacer que el número de contrato que se inserte sea la suma del número de contrato en contratos 2 más el mayor número de contrato existente en la tabla contratos (supongamos que es el 24).

```sql
INSERT INTO contratos(numcontrato, matricula,dnicliente, fini, ffin, kini, kfin) SELECT numcontrato+24, matricula,dnicliente, fini, ffin, kini, kfin FROM contratos2;

INSERT INTO contratos(numcontrato, matricula,dnicliente, fini, ffin, kini, kfin)SELECT numcontrato+(select max(numcontrato) from contratos), matricula,dnicliente, fini, ffin, kini, kfin FROMcontratos2;
```

**Ejemplo 9:** El cliente con dni 08785691K ha comunicado que hoy mismo quiere alquilar todos los automóviles de la marca Seat que no estén alquilados actualmente. Insertar los nuevos contratos para ese cliente con la fecha actual y las matriculas de esos automóviles. 
Se insertará en kilómetros iniciales los kilómetros registrados en la tabla automóviles y se comprobará que no estén alquilados.

```sql
INSERT INTO contratos(matricula,dnicliente, fini, kini) 
SELECT matricula, '08785691K',curdate(),kilometros FROM automoviles WHERE alquilado=false AND marca='seat';
```

**Ejemplo 10:**  Mariano Dorado ha comunicado que hoy mismo quiere alquilar todos los automóviles de precio inferior a 70€ que no estén alquilados actualmente. Insertar los nuevos contratos para ese cliente suponiendo que no podemos conocer su dni primero para después usarlo en INSERT.

```sql
INSERT INTO contratos (matricula,dnicliente, fini, kini) 
SELECT matricula, dni,curdate(),kilometros FROM automoviles,clientes WHERE alquilado=false AND precio<70 AND nombre='mariano' AND apellidos='dorado’;
```

ES UNA CONSULTA CON PRODUCTO CARTESIANO, HACE TODAS LAS COMBINACIONES DE LOS COCHES CON EL CLIENTE.

**Ejemplo 11:** Supongamos que en la base de datos LIGATERCERA tenemos una tabla CALENDARIO(eqLocal, eqVisitante, numjornada, numpartido,fecha) donde la clave principal está formada por las dos primeras columnas (los códigos del equipo local y del visitante) y las otras columnas admiten nulos, insertar todos los posibles enfrentamientos o partidos entre los equipos de la liga.

```sql
INSERT INTO calendario  (eqLocal, eqVisitante) 
SELECT a.codeq, b.codeq FROM equipos AS a, equipos AS b WHERE a.codeq!=b.codeq;
```

## HOJAS DE EJERCICIOS

💻 Hoja de ejercicios 1.

💻 Hoja de ejercicios 2.

## 2.- ACTUALIZACIÓN DE DATOS. LA INSTRUCCION UPDATE.

La instrucción para realizar modificaciones de los datos es **UPDATE**. La sintaxis de UPDATE es:

```sql
UPDATE [IGNORE] tabla |  combinación_de_tablas  
SET   columna1=expresión1, columna2=expresión2, ..... 
WHERE condición;
```

- Tras UPDATE, se escribe la tabla en la que se van a actualizar datos o, en su caso, la combinación de tablas afectadas por la operación.

- Modifica los valores de las columnas indicadas en SET con el resultado de las expresiones. Si se usa WHERE, sólo se modifican las filas que cumplen la condición.

- Dentro de la sintaxis, incluso se permite usar ORDER BY para establecer el orden en que se van modificando datos de filas.

Condiciones de la sintaxis y ejecución de UPDATE.

1. Detrás de UPDATE se puede especificar:
El nombre de una tabla si la modificación sólo afecta a esa tabla y está condicionada al contenido de esa tabla.
La combinación de varias tablas cuando la modificación afecta a dos o más tablas relacionadas o bien está condicionada al contenido de varias tablas.

2. Detrás de SET se especifican los valores que se van a asignar a las columnas que se quieren modificar. Estos valores pueden ser:
    - Valores constantes.
    - El resultado de una expresión que utiliza o no los valores almacenados en otras columnas o en la misma columna.
    - El resultado de una función.
    - El resultado de una subconsulta.

3. Si para calcular una expresión, se usa una columna que también se modifica, el valor que se usa es el almacenado antes de hacer la modificación.

4. Es indiferente el orden en el que se especifiquen las columnas a modificar puesto que las modificaciones se ejecutan calculando primero los nuevos valores que va a haber en una fila y modificando después el contenido de la fila con esos valores.
 
5. Las expresiones deben ser calculables a partir de los valores correspondientes a la fila que se esté modificando.

6. Con la cláusula WHERE se indica la condición que deben cumplir las filas que se van a modificar. Si no se usa WHERE, la modificación afectará a todas las filas de la tabla. 

7. Al modificar el contenido de una columna, está columna deberá cumplir todas las condiciones de dominio, clave principal, clave ajena e integridad referencial que se tengan que cumplir en función de las restricciones establecidas en las tablas de la base de datos.

8. Si se modifica una clave principal que tiene relación de integridad referencial con actualización en cascada con respecto a una clave ajena en otra tabla, el valor de la clave ajena se modifica con el nuevo valor de la clave principal en todas las filas relacionadas.

9. La cláusula IGNORE no aborta el proceso de actualización cuando se tratan de actualizar varias filas y algunas no pueden actualizarse por conflictos de clave primaria, clave ajena, errores de conversión de datos, etc. Las que producen errores no se actualizan y si que se actualizan las que no los producen. Si no se usa IGNORE y alguna fila produce error,  se rechaza por completo la actualización.

**Ejemplo 1:** Modificar el contrato número 19 para que contenga fecha final 14 de enero de 2020 y kilómetros finales 48111. Comprueba previamente el contenido del contrato 19.

```sql
UPDATE contratos set ffin='2020-01-14', kfin=48111 WHERE numcontrato=19;
```

Comprueba el contenido del contrato 19 tras ejecutar UPDATE.

¡¡¡Hay que tener mucho cuidado al probar o realizar estas instrucciones. Si en este caso no hubiéramos escrito la condición WHERE, modificaríamos las fechas y kilómetros de todos los contratos existentes. !!!

**Ejemplo 2:** Modificar la columna alquilado del vehículo 7839JDR para que indique que está disponible.

```sql
UPDATE  automoviles SET alquilado=false WHERE matricula='7839JDR';
```

**Ejemplo 3:** Modificar la columna alquilado del vehículo 7839JDR para que contenga lo opuesto a lo que contenía.

```sql
UPDATE  automoviles SET alquilado=NOT alquilado WHERE matricula='7839JDR';
```

**Ejemplo 4:** Modificar la columna precio de todos los automóviles de precio de alquiler superior a 100 euros para que su precio se reduzca en un 30% y en un 25% el de los de alquiler inferior a 100 euros. El precio debe quedar con dos decimales. 

Ojo, dado que hay que reducir precios, es importante reducir primero los de precio inferior. Si no se hace así, puede que reduzcamos el precio dos veces para algún coche.

```sql
UPDATE automoviles SET precio=round(precio*0.75,2)  WHERE precio<100;
UPDATE automoviles SET precio=round(precio*0.7,2)  WHERE precio>=100;
```

Se podría dar una solución con una sola instrucción usando la función IF:

```sql
UPDATE automoviles 
SET precio=if(precio<100,round(precio*0.75,2), round(precio*0.7,2));
```

**Ejemplo 5:** Antes de realizar la siguiente instrucción comprueba cuales son los contratos realizados para el coche de matrícula 3273JGH. Modificar la matrícula del automóvil de matrícula 3273JGH para que tenga la matrícula 3233JMG.

```sql
UPDATE automoviles SET matricula='3233JMG' WHERE matricula='3273JGH';
```

En este caso, dado que la columna matricula de la tabla contratos es clave ajena o FOREIGN KEY relacionada con las matricula de la tabla contratos con restricción de integridad referencial por actualización en cascada, además de haberse modificado la matrícula en la tabla automóviles, se habrá modificado la matrícula en todos los contratos correspondientes a ese automóvil.

**Ejemplo 6:** Modificar las fechas de los contratos para que en todos aquellos que tengan una fecha inicial superior a la fecha actual, se le reste un año en la fecha que tienen.

```sql
UPDATE contratos SET fini=subdate(fini,INTERVAL 1 YEAR) WHERE fini>curdate();
 ```

**Ejemplo 7:** Modificar las fechas de los contratos para que en todos aquellos en los que la fecha final sea inferior a la inicial, se intercambie el valor de esas fechas.

```sql
UPDATE contratos SET fini=ffin, ffin=fini WHERE fini>ffin;
```

## HOJAS DE EJERCICIOS

💻 Hoja de ejercicios 3.

## 3.- ELIMINACIÓN DE FILAS. LA INSTRUCCIÓN DELETE

La instrucción para eliminar o borrar filas en tablas es **DELETE**. La sintaxis de DELETE es:

```sql
DELETE [IGNORE] [tabla1,  ...]  
FROM {tabla | combinacion de tablas}
[WHERE condicion] [ORDER BY criterio] [LIMIT num_filas]
```

O bien 

```sql
DELETE [IGNORE] [tabla1,  ...]  
USING {tabla | combinacion de tablas}
[WHERE condicion] [ORDER BY criterio] [LIMIT num_filas]
```

En la instrucción se indica la tabla o las tablas en las que se eliminan filas. Se eliminan las filas resultado de la combinación de tablas que se indica tras FROM o USING que cumplan, en su caso, la condición WHERE.

Condiciones de la sintaxis y ejecución de DELETE.

1. Detrás de DELETE se indica la tabla o las tablas en las que se eliminan filas (la combinación de tablas se suele usar cuando la condición where depende del contenido de otra). Lo normal es que se eliminen filas de una sola tabla. Es muy raro necesitar eliminar filas de más de una tabla con una instrucción DELETE.

2. Detrás de FROM o USING se indica la tabla o la combinación de tablas (INNER JOIN, LEFT JOIN, producto cartesiano) sobre las que se va a condicionar el borrado o eliminación. Necesariamente la tabla de la que se eliminan filas tiene que formar parte de la combinación de tablas.

3. Si se eliminan filas de una tabla que cumplen determinadas condiciones en esa tabla, no habría combinación de tablas detrás de FROM y la sintaxis sería:

```sql
DELETE FROM tabla WHERE …..
```

Por tanto, tras DELETE no se escribe el nombre de la tabla de la que se eliminan los datos. Se eliminan de la tabla que se indica tras FROM.

4. Si no se usa WHERE, se borrarán todas las filas de la tabla. 

5. Se puede usar ORDER BY para determinar el orden en el que se van eliminando las  filas y LIMIT para establecen que no se eliminen más filas que las indicadas en esa cláusula.

6. Una vez eliminadas las filas, ya no se pueden recuperar, salvo que hayamos hecho la eliminación dentro de una TRANSACCIÓN.

7.  Si la tabla donde se realiza la eliminación está relacionada con otra u otras tablas con integridad referencial, se cumplirán las reglas de eliminación aplicadas. Si está establecida la condición NO ACTION o la condición RESTRICT, no se permite el borrado en la tabla principal si tiene filas relacionadas en la tabla relacionada. Si admite la eliminación en cascada, se eliminan las filas de la tabla principal y todas las filas relacionadas de la tabla relacionada.

8. La cláusula IGNORE hace que se ignoren los errores de borrado en una instrucción que borre varias filas. Si se usa IGNORE y alguna de las filas a eliminar no se pudiera eliminar, no se abortaría toda la eliminación, se eliminarían todas las que no causan error. El sistema nos daría avisos warning.


**Ejemplo 1:** Eliminar todas las filas de la tabla contratos2.

```sql
DELETE   FROM  contratos2;
```
 
**Ejemplo 2:** Eliminar en la tabla clientes el cliente con dni 08785691K.

Aquí, además de eliminar el cliente con ese dni, podrían eliminarse filas en la tabla relacionada contratos si hubiera entre las dos tablas relación de clave ajena con integridad referencial con eliminación ON CASCADE, o no permitirse la eliminación del cliente si está establecida la restricción de eliminación NO ACTION o la restricción  RESTRICT.

Comprueba cual es la regla de integridad referencial en la relación FOREIGN KEY entre contratos y clientes y comprueba lo que va a ocurrir.

```sql
DELETE  FROM clientes WHERE  dni=’08785691K’;
```

**Ejemplo 3:** Eliminar todos los contratos realizados hoy.

```sql
DELETE FROM contratos WHERE fini=curdate();
```

**Ejemplo 4:** Eliminar todos los contratos terminados hace más de un año.

```sql
DELETE FROM contratos WHERE ffin<date_sub(curdate(),INTERVAL 1 YEAR);
```
**Ejemplo 5:** Vamos a ver cómo se pueden ejecutar instrucciones, en este caso de eliminación de filas, dentro de una transacción y tras detectar que hemos eliminado datos accidentalmente, los podemos recuperar anulando la transacción.

1. Inicia una transacción.

```sql
START TRANSACTION;
```

2. Elimina el último contrato de la tabla contratos.

```sql
DELETE FROM contratos ORDER BY numcontrato DESC LIMIT 1;
```

3. Ahora queremos eliminar los contratos realizados hoy y ejecutamos accidentalmente.

```sql
DELETE FROM contratos WHERE fini;
```
4. Obtenemos el contenido de la tabla contratos y nos damos cuenta que nos hemos cargado todos los contratos.
5. Podemos volver al punto en el que se encontraba la base de datos antes de comenzar la transacción anulando ésta con la instrucción:

```sql
ROLLBACK;
```

6. Si todo hubiera ido bien, en lugar de ROLLBACK, habríamos ejecutado COMMIT para confirmar todo lo realizado durante la transacción.

**Ejemplo 5:** Ahora vamos a usar una combinación de tablas para condicionar las filas que se eliminan en una tabla.

Realiza el ejemplo dentro de una transacción para así no perder información en la base de datos. Se trata de un ejemplo y únicamente queremos ver que funciona, no deseamos eliminar realmente esas filas.

Elimina todos los contratos realizados y por el cliente de nombre Carlos Javier y apellidos Lopez Carvajal. Fíjate que se elimina en contratos y que la condición de eliminación se establece sobre datos de la tabla clientes.

```sql
DELETE contratos FROM contratos INNER JOIN clientes ON dnicliente=dni WHERE nombre='carlos javier' AND apellidos='lopez carvajal';
```

**Ejemplo 6:**  Elimina todos los contratos realizados por Mariano Dorado y los datos de ese cliente. Se podría hacer lo siguiente si hubiera relación de integridad referencial con regla de borrado en cascada entre clientes y contratos. Al eliminar el cliente se eliminarían todos sus contratos.

```sql
DELETE FROM clientes WHERE nombre='mariano' AND apellidos='dorado';
```

Pero eso no se puede hacer ya que no hay esa regla, se trata de eliminar un cliente que tiene contratos y el servidor no lo permite ya que hay restricción de borrado NO ACTION. Tampoco se permitiría esto por la misma razón.

```sql
DELETE clientes,contratos FROM contratos INNER JOIN clientes ON dnicliente=dni WHERE nombre='mariano' AND apellidos='dorado';
```

Por tanto, hay que eliminar primero los contratos y después el cliente:

```sql
DELETE contratos FROM contratos INNER JOIN clientes ON dnicliente=dni WHERE nombre='mariano' AND apellidos='dorado';
DELETE FROM clientes WHERE nombre='mariano' AND apellidos='dorado';
```

## HOJAS DE EJERCICIOS

💻 Hoja de ejercicios 4.

💻 Hoja de ejercicios 5.

💻 Hoja de ejercicios 6.

## ACTIVIDAD GRUPAL

💻 Crisis en la empresa. Parte 1.

💻 Crisis en la empresa. Parte 2.

