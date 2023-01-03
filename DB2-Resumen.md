DB2 SQL tiene 4 tipos de operanciones. 

DDL (data definition language) Que tienen 3 tipos de statements 
Create (Table, view,synonym,alias) 
Alter (Table, view,synonym,alias) 
Drop (Table, view,synonym,alias) 

DASD son grupos de databases

DML (data manipulation language) 4 statements
Insert Into nombre-tabla Values(value1,value2,...)
Update Table nombre-tabla SET COLUMN=val(lugar-dela-columna=val-id)
Delete Table nombre-tabla
Select (*/column1/column2,column4/...) FROM ...

DCL
GRANT 
REVOKE

TCL
Commit sirve para guardar los cambios
rollback para deshacer los cambios

Tipos de data

Numeric:
Integer(INT) 4bytes Se usa con numeros grandes
SMALLINT 2bytes Se usa con numeros más chicos que 32.000
Decimal Sirve para cuando tenemos numeros con coma
single floating point/real raramente usado
double floating point raramente usado

String:
CHAR ocupa 4 bytes su largo es exacto
VARCHAR puede variar su largo

DATETIME:
DATE Tiene dia mes y año 
TIME hora minutos y segundos
TIMESTAMP condensa todo lo anterior más huso horario 
 
Boolean sirve para indicar verdadero o falso 0 o 1

Para crear una tabla debemos en un archivo SPUFI o QMF:

CREATE TABLE nombre-tabla(CAMPO_1 INTEGER, CAMPO_2 CHAR(X), CAMPO_3 SMALLINT, CAMPO_4 DATE, CAMPO_5 DECIMAL(10,2),...);

CREATE TABLE nombre_tabla LIKE tabla_original;  Sirve para crear una tabla con campos iguales a otra 

Para ejecutar esta tabla tenemos que usar ;;;

Si no tenemos acceso debemos debajo de la tabla a crear poner:
IN nombredatabase.nombretablespace;

Para crear un comentario debemos -- doble guion 

Crear un view (sirve para extraer detalles importantes de la tabla y mantenerlo en una tabla virtual)
CREATE VIEW nombreview AS SELECT campo_1,campo_2 from tabla1, tabla2,...;

SELECT * nombreview; (sirve para ver el view de arriba)


ALTER Sirve para modificar diferentes campos de una tabla ya creada

ALTER TABLE nombre_tabla ADD nombre_campo VARCHAR(x); (Agrega una columna nueva)

ALTER TABLE nombre_tabla ALTER nombre_campo SET DATA TYPE ...; (cambia el tipo de data type de un campo, de char a varchar por ejemplo)

ALTER TABLE nombre_tabla RENAME COLUMN nombre_campo TO nombre_nuevo; (cambia el nombre de un campo)


DROP Se usa para eliminar una tabla permanentemente. Elimina TODO

DROP TABLE nombre_tabla;


INSERT Sirve para insertar o copiar datos a una tabla.

INSERT INTO nombre_tabla VALUES (XXXX,"XXXX","XXXX-XX-XX",...); Sirve para insertar data en una tabla. 

INSERT INTO nombre_tabla (nombre_campo1,nombre_campo2) VALUES (xxxx,XXXxx); Sirve para insertar data en una tabla en columnas especificas

INSERT INTO nombre_tabla SELECT * FROM otra_tabla; Sirve para copiar todas las columnas de una tabla a otra
Se pueden copair columnas especificas de la siguiente manera: 
INSERT INTO nombre_tabla (campo1,campo3)  SELECT campo1,campo3 FROM otra_tabla; 

INSERT INTO nombre_tabla


SELECT sirve para ver records de la tabla

SELECT (* / CAMPO1,CAMPO2,...) FROM nombre-tabla [WHERE cond] [GROUP by colm][HAVING group-cond][ORDER BY colm-name];
Todas las posibles variables con select 

SELECT * FROM nombre_tabla; selecciona toda la tabla
SELECT campo1,campo2 FROM nombre_tabla; Selecciona columnas en especifico
SELECT campo1 FROM nombre_tabla WHERE campo1= ò < ò > x; Selecciona el o los campos que sean mayor, menor o igual que cierto número. 
SELECT campo1 FROM nombre_tabla WHERE campo1 IN (xxxxx,xxxxxx); Selecciona solo a los records que le pedís 
SELECT campo1 FROM nombre_tabla WHERE campo1 BETWEEN 00000,99999; Selecciona los records que se encuentran entre esos numeros
SELECT campo1 FROM nombre_tabla WHERE campo1 LIKE "_xxxx%"; Selecciona valores alfabeticos que contengan ese Like. El guion bajo se usa para indicar que puede ser cualquier valor 

SELECT campo1 FROM nombre_tabla WHERE campo1 NOT LIKE "xxx%"; Selecciona todo lo que no sea como este valor

SELECT campo1 FROM nombre_tabla WHERE EXIST (SELECT campo1 FROM nombre_tabla WHERE campo1=9999); Se fija si la condicion se cumple y si lo hace muestra todos los campos sino no muestra nada.  

SELECT campo1,sum(campo2) FROM nombre_tabla WHERE campo1=xxxx GROUP BY campo1; Selecciona todos los campos1 que sean igual al WHERE y hace una cuenta, en este caso suma. 

SELECT campo1,sum(campo2) campo_nuevo FROM nombre_tabla GROUP BY campo1; En este caso muetra todos los campo1 y los suma en campo_nuevo.  

SELECT campo1 COUNT(*) nuevo_campo_cuenta FROM nombre_tabla GROUP BY campo1; En este caso suma la cantidad de campo1 existentes.+

SELECT campo1 FROM nombre_tabla ORDER BY campo1; Selecciona campo1 y lo ordena de manera ascendente. Para hacerlo de manera descendiente debemos poner la palabra DESC al final. 



UPDATE Sirve para poner un valor o cambiar un valor existente. 

UPDATE nombre_tabla SET campo1=XXXX WHERE campo_id=yyyy; Pone un valor nuevo en campo1 usando campo_id como ubicación.


DELETE se usa para borrar un record de la tabla

DELETE FROM nombre_tabla; Borra TODOS los campos de una tabla 

DELETE FROM nombre_tabla WHERE campo1=xxxx; Busca campo1 y borra todos sus datos. 
