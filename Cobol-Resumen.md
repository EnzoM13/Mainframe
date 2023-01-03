IDENTIFICATION DIVISION.
sirve para incluir en ella información como del programa como el nombre, el autor y otros datos relacionados.
ENVIRONMENT DIVISION.
es la que contendrá información del entorno, sobre el ordenador en el que se ha escrito, el ordenador donde se va a ejecutar, etc. (no es obligatoria)
DATA DIVISION.
es una de las divisiones más importantes pese a ser opcional. En ella se escriben registros, variables, nombres de campos, etc.
WORKING-STORAGE-SECTION.
acá es donde se escriben las variables. se debe empezar con el nivel ej: 01
PICTURE IS (PIC) sirve para indicar si la variables es un número o un string
Para insertar un string alfabetico se debe usar la letra A.
Para insertar un string alfanumerico se debe usar la letra X seguido de un () con la cantidad de caracteres 
Para insertar un numero se debe usar el 9 seguido de un () con la cantidad de caracteres 
Para converir un numero a decimal se debe usar la letra V ej: num1 pic 99V99.
Para usar numeros negativos debemos poner una S delante del 9 ej: num1 pic S99.
Estos se pueden combinar ej: num1 pic S99V99
Para dejar vacios en los displays que podrían ser 0 se debe usar la Z ej: num1 PIC Z99 VALUE 17. Se mostrará como 17 y NO como 017 sin embargo quedará el espacio vacio delante del numero
Para hacer un Caracteres de edición decimal se debe hacer así ej: num1 PIC 99.99 VALUE 50.75. Sin embargo este valor no se comportará como numero y solo servirá para demostración. 
VALUE se usa para definir el valor de una variable
SPACE o SPACES. Inserta uno o dos valores en blanco a una variable ej. texto PIC XX VALUE SPACE/S. Es recomendable darles valor siempre. 
ZERO ZEROS ZEROES Igual que space pero para numeros. ej: num1 PIC 99 VALUE ZERO. 
 
PROCEDURE DIVISION
es la que contendrá los procedimientos necesarios para que el programa funcione con los datos de la DATA DIVISION.

Para nombrar "palabras", variables, o constantes solo se pueden usar letras, numeros y guion medio.
Cobol no es keysensitive y siempre se tiene que terminar con un punto (.) similar al punto y coma actual

DISPLAY "xxxx".  Es igual el print actual
MOVE inserta un dato dentro de una variable
para insertar un dato en más de una variable a la vez se puede hacer dejando un espacio.ej MOVE 10 TO num1 num2 num3.
STOP RUN. termina el programa
ACCEPT es igual a input	
para hacer una operación matemática deberíamos hacer por ejemplo:
	ADD num1 TO num2 GIVING resultado.  
ADD sirve para sumar y se tiene que juntar con TO
SUBTRACT sirve para restar y se tiene que juntar con FROM
MULTIPLY sirve para multiplicar y se tiene que juntar con BY
DIVIDE sirve para dividir y se tiene que juntar con BY
GIVING sirve para darle un valor a una variable 
REMAINDER es para obtener el resto de una division.
INSPECT nombre_string TALLYING FOR CHARACTERS. Sirve para contar la cantidade letras.
INSPECT nombre_string TALLYING FOR ALL "x". Sirve para contar la cantidad de una letra en específico. 
DISPLAY FUNCTION UPPER-CASE(nombre_string) todo en mayus
DISPLAY FUNCTION LOWER-CASE(nombre_string) todo en minus
STRING string1 DELIMITED BY SIZE SPACE string2 DELIMITED BY SIZE INTO string3.
Fusiona string1 con string2 y se almacena en string3. Delimited by size sirve para que capture todo el string 
mientras que si usamos DELIMITED BY SPACES agarrá hasta el primer espacio DELIMITED BY "x" hace que agarre hasta esa letra o caracter. 
ON OVERFLOW. 
IF el punto se pone al finalizar el if No en el primer renglon
AND OR NOT o IS NOT se pueden usar para marcar cosas dentro del if. Se pueden agrupar más  de una y es recomendable usar parentesis para que sea más legible. ej:
IF (num1 = 15 OR <15) AND (num2 NOT = 15 OR <15) THEN...
ELSE
END-IF. 
PERFORM sirve para hacer rutinas, para llamar funciones
GO TO sirve también para llamar funciones 
La diferencia entre perform y go to es que perform salta para luego volver al renglon de abajo, mientras que go to salta sin volver. 
MOVE
COMPUTE
PERFORM ... THRU para llamar a dos variables a la vez en un perform. ej: PERFORM nombre THRU apellido. 
PERFORM ... TIMES sirve para asignar cuanta cantidad de veces quiero que se haga una funcion ej: PERFORM calculo 10 TIMES.
PERFORM ... UNTIL sirve para decir que haga algo hasta que la variable valga X similiar a times pero más especifica podiendo usar variables. ej: PERFORM calculo UNTIL numero=100.
PERFORM VARYING varying es capaz de incrementar y decrementar valores en las variables. varios ej:
PERFORM CONTAR VARYING NUMERO 1 BY 1 UNTIL NUMERO=100
el primer numero de by es desde donde empieza a contar y el segundo de cada tantos numeros suma. ej 10 by 5 empieza a contar desde diez y cuenta de 5 en 5. 
END-PERFORM

Variables compuestas
Para crear una es necesario dejar la primera variable solo con el nombre y abajo colocar las subvariables. EJ
01	Variable-compuesta.
	02 Num1 pic 9(3) value 125.
	02 Num2 pic 9(3) value 135.
Se puede hacer display de toda la variable o solo de las subvariables.
Numeros de nivel de variable.
01 Es para asignar un primer valor a una variable
02 al 49 son para las subvariables 
66 renombrar clause items 
77 es para variables que nunca se van a subdividir
88 identifica posibles valores que se pueden almacenar en una variable esta acepta PICs ej:
 01	EDAD PIC 999.
 	88 NIÑO VALUE 1 THRU 18.
	88 ADULTO VALUE 19 THRU 65.
	88 ANCIANO VALUE 66 THRU 100.

FILLER sirve para crear una variable que no puede ser cambiada ej. FILLER PIC X(10) VALUE "HOLA QUE TAL". por más que usemos MOVE no va a cambiar su valor. 

Estructuras anidadas de variables
Se pueden crear sub variables dentro de otras sub variables. ej:
01	variable-compuesta.
	02 texto1 PIC X(4).
	02 texto2 PIC X(5).
	02 sub-variable-compuesta.
		03 texto3 PIC X(7).
		03 sub-sub-variable-compuesta.
			04 texto4 PIC X(4).
Algunas reglas a seguir es que el numero de identificación siempre tiene que ir creciendo y se pueden definir hasta 7 niveles anidados

BASE DE DATOS

Dentro de la DATA DIVISION, FILE SECTION se crean los campos de una base de datos. Para comenzar una debemos utilizar la palabra reservada FD ej:
FD	empleados-archivo.
	01 empleados-registro.
		02 empleados-id PIC X().
		02 empleados-nombre PIC X().
		02 empleados-apellido PIC X().
Es una buena practica nombrar a todas las variables con el mismo comienzo para esa base de datos.
Para crear un archivo físico debemos en ENVIROMENT DIVISION, FILE-CONTROL. usar el comando SELECT OPTIONAL para remitirnos a la base de datos y luego usamos un assing para crear un archivo. ej:
SELECT OPTIONAL empleados-archivo.
ASSIGN TO "empleados.dat"
ORGANIZATION IS LINE SEQUENTIAL. 

Cuando colocamos ORGANIZATION IS LINE SEQUENTIAL significa que ordenará a la base de datos de la primera entrada a la última. 

OPEN se usa para abrir un archivo de base de datos, se utiliza en en PROSEDURE DIVISION. 
OPEN EXTEND hace que si el archivo especificado existe se abra y se pueden añadir nuevos valores, si no existe el archivo lo crea y luego añade los valores. 
OPEN I-O si el archivo existe se puede abrir y leer pero si no existe produce un error
OPEN INPUT si no existe un archivo da error. 
OPEN OUTPUT si el archivo existe lo reemplaza y pone nuevos registros pero si no existe lo crea 
La palabra OPTIONAL se usa para que cree un archivo si no existe uno cuando usamos I-O o INPUT
CLOSE cierra el archivo que abrimos con OPEN 
WRITE se usa para escribir en los archivos abiertos de bases de datos 
REWRITE sirve para actualizar un archivo
READ lee los archivos
NEXT RECORD  indica que se va a leer el siguiente registro 
PREVIOUS RECORD leería el registro anterior
AT END sirve para leer el archivo hasta el final 
Modos de acceso
SEQUENTIAL del primero al último 
RANDOM se desplaza directamente al registro que necesitemos
DYNAMIC intermedio entre los dos ultimos. 
RECORD KEY variable. Crea una clave primaria en la base de datos. 

ORGANIZATION IS INDEXED RECORD KEY IS x ACCESS MODE DYNAMIC. solo funciona con OPEN I-O o OUTPUT

ALTERNATE RECORD KEY. Clave alternativa. Se refiere a otro campo numérico o alfanumérico para poder darle otro orden al fichero a la hora de consultar
 Si queremos una clave alternativa que se pudiera repetir hay que indicarle la cláusula WITH DUPLICATES.


COPY "ruta-del-archivo.cob"
INVALID KEY sirve para decirle a cobol que hacer si no se encuentra un registro.
WITH LOCK sirve para bloquear el archivo mientras se usa.

EVALUATE TRUE similar a ELIF se usa con WHEN y WHEN OTHER y termina con END-EVALUATE.

REDEFINES sirve para cambiar un Integer a un string ej:
77	NUM1 PIC 9.
77	NUM2 REDEFINES NUM1 PIC X. 
Tienen que ir una abajo de la otra sino tira error.

WITH NO ADVANCING sirve para saltar un renglon y luego usarlo ?

SET x TO TRUE o SET x TO FALSE.

Reports.
Similar a crear un archivo, tienen su propia extensión .rpt ej:
FILE-CONTROL.
SELECT report-name ASSING TO "report-file.rpt" ORGANIZATION IS LINE SEQUENTIAL.

FILE SECTION. 
FD report-name.
01
	02 
Tablas.
Para crear una tabla debemos posicionarnos en la WORKING-STORAGE-SECTION.
Paso siguiente creamos la tabla ej:
01 Tabla1.
	02 Clientes OCURRS x TIMES. Podemos indicar la cantidad de filas que queremos en nuestra tabla.
		03 Nombre-cliente PIC X(12).
		03 Apellido-cliente PIC x(12). Acá estamos creando los cabezales de las columnas. 

Se pueden crear tablas indexadas. ej:
01 Tabla2
	02 Producto OCURRS 2 TIMES INDEXED BY x.
		03 Nombre-producto PIC X(10).
		03 Tamaño-producto OCURRS 3 TIMES INDEXED BY xx. 
			Tamaño-tipo PIC A.
Tablas prerellenas.
REDEFINES te deja prerellenar las tablas

SD
SORT
MERGE 
