Job statements:
JOB informacion del trabajo como nombre del trabajo, del programa, id del programador etc
EXEC especifica el programa o procedimiento a usar
DD especifica los archivos a utilizar en el trabajo
Parametros:
Positional -> aparecen en un orden y posicion especifico
Keyword Pueden aparecer en cualquier orden o posicion 
Para crear un jcl siempre tiene que tener record length 80, siempre tiene que estar en mayus 
Sintaxis de un JCL
//NAMEFIELD OPERAND PARAMETERS
Para empezar tenemos que usar un identifier //, luego ponemos el nombre de nuestro programa si es un job, luego aclaramos el statement y al final los parametros. Los parametros son el userid y puede tener el sector de donde se trabaja. ej: //SORTJOB JOB (A123456,"HR"). Los parametros deben ir en parentesis. Si no se pone ningun parametro, se debe usar una coma. ej: //SORTJOB JOB ,
JOB
Osea en los parametros primero debemos dar la account info, después nombre de programador y ahora podemos usar los keywords.
Keywords:
CLASS= Se usan para definir que tipo de programa o funcion estamos realizando. Puede ir de la a-z y 0-9 
PRTY= Puede ser del 0-15. 15 es la prioridad más alta.
NOTIFY= Sirve para avisar a algun usuario que ha terminado el trabajo. Debemos colocar el id de la persona que qeuremos notificar. &sysuid signfica que la persona que haga este trabajo será notificada
MSGCLASS= Puede ser de a-z y 0-9 Especifica el output destinatario del trabjo ej si ponemos P sigifica que queremos que imprima el trabajo
MSGLEVEL=(STATE,MSG)  Se definen dos numero que pueden ir del 0-2
Statement 0 job statements solo
	  1 JCL con parametros simbolicos expandido
	  2 JCL solo
MENSAJE	  0 Mensajes escritos solo con terminacion abnormal
	  1 Mensajes escritos independientemente de si sean de terminacion normal o anormal.

TYPRUN= Puede ser scan o hold. Scan significa buscar errores en el trabajo pero no compilarlo. Hold sirve para que no se haga el trabajo. Hold lo pone en la cola y puede ser ejecutado después 
TIME=(mm,ss) Sirve para decir por cuanto tiempo queremos que dure el programa en minutos y segundos.
REGION= Puede ser nK o nM. Sirve para decirle al programa cuanto espacio vamos a alocar.  Ej: REGION=4M
Para hacer un comentario debemos usar * puede ser //* o /*, es indiferente
Para terminar el trabajo debemos tipear /*

EXEC
PGM= significa que queremos ejecutar un programa
PROC= Significa que queremos hacer un proceso

Sintaxis -> //STEPNAME EXEC PARAMETERS
PARM= se utiliza para pasar valor al programa que está siendo ejecutado
ACCT= especifica la informacion contable del paso de trabajo.
TIME=(mm,ss) cuanto tiempo puede tardar un paso
REGION=

DD
Sintaxis -> //DDname DD Parameters
* 
DUMMY
DATA

DSN= Es el nombre del dataset que vamos a usar.
DISP= Sirve para indicar el estatus del dataset. Si es NEW significa que se creará un nuevo dataset. Si es OLD significa que usará un dataset ya creado, informacion existente será sobreescrita. Si es SHR el dataset ya existe y la diferencia con old es que multiples jobs pueden acceder a el al mismo tiempo. Si es MOD el dataset si no existe se crea y sino lee el antiguo, la informacion nueva la guarda al final. 
Pueden ser normal end: CATLG, UNCATLG, DELETE, KEEP PASS.
O pueden ser abnormal end: CATLG, UNCATLG, DELETE, KEEP.
	CATLG se retiene el dataset con una entrada en el catalogo
	UNCATLG se retiene el dataset pero la entrada al catalogo del sistema se borra
	KEEP el dataset se retiene sin cambiar el catalogo
	DELETE el dataset se borra del usuario y del catalogo
	PASS Sirve para cuando el dataset tiene que pasar al proximo paso de trabajo.
DCB=(LRECL, RECFM, BLOCKSIZE, DSORG) Se utiliza cuando se crea un nuevo dataset para dar parametros
Ej: DCB=(LRECL=80,RECFM=FB,BLKSIZE=800,DSORG=PS)
SPACE=(Spaceunits, (primary, secondry, directory-blocks)RLSE) Es usado cuando estamos creando un nuevo dataset.+
Ej: SPACE=(TRACKS,(100,100),RLSE) 
UNIT= Donde queremos guardar nuestro dataset sea DASD o un disco SYSDA
VOL=SER= Es un numero que le asignamos de volumen
SYSOUT= es lo que tiene que hacer al terminar el trabajo. Si ponemos A significa que va a impresion. Si ponemos * copia a MSGCLASS

Librerias 
JOBLIB 
JCLLIB
STEPLIB

Para utilizar codigo repetitivo tenemos que usar la palabra PROC, de procedure. Debemos nombrar a ese proc para luego llamarlo al querer utilizarlo. Al terminar utilizamos la palabra PEND. Para no repetir nombres de ps o psd dentro de un PROC, debemos usar el codigo DSN=&DNAME, Ej:

//NOMBRE-PROC PROC 
//PASO1 EXEC PGM=x 
//DD1 DD DSN=&DNAME
//
//PEND

//STEP2 EXEC NOMBRE-PROC, DNAME=X 

& Se puede usar con cualquier palabra exepto las reservadas. Se puede tambien indicar la posición ej: 
DSN=CLIENT.LIST.&LASTN

LASTN=PS1 etc.

Se pueden invocar partes de codigo escrito en otros archivos con la palabra JCLLIB ej:

//NOMBRE1 JCLLIB ORDER=NOMBRE.DATASET.JCLLIB
//PASO2 EXEC NOMBRE-PS, NOMBRE&=X
Se puede hacer esto varias veces vinculando codigo dentro de los otros datasets ifinitamente.



Para usar un exec que no esté en sys1, tenemos que linkearlo con JOBLIB ej:
//JOBLIB DD DSN=RUTA.DEL.PROGRAMA, DISP=SHR

STEPLIB hace el mismo trabajo que JOBLIB pero solo lo aplica a un EXEC, mientras que JOBLIB lo aplica a todo el trabajo. EJ:
//STEPLIB DD DSN=RUTA.DEL.PROGRAMA, DISP=SHR

COND parameter

COND=(Returncode, operator, stepname)
Operator puede ser:
GT greater than
EQ equal to
GE greater than or equal to
LT less than
NE not equal
LE less than or equal to

Ej:
COND=(0,EQ,NOMBREDELPASO)

Si da falso el condicional se ejecuta el siguiente trabajo sino lo salta.
Se puede usar en el JOB del principio para que chequee el condicional en cada paso. ej: //Nombrejob JOB a, COND=(0,LT)
Para saltarse siempre un paso y que no se ejecute tenemos que usar el COND=(0,LE) Ya que siempre dará verdadero y por lo tanto no se ejecutará esa linea
Para siempre ejecutar un paso debemos COND=(4095,LT) Esto siempre será falso y el paso siempre se ejecutará
COND=ONLY sirve para que si los anteriores pasos se ejecutaron correctamente este NO lo hará.
COND=EVEN sirve para que si los anteriores pasos se ejecutaron correctamente este lo haga también.
Tambien se puede codear de la siguiente manera
NOMBREIF IF RC(osea return code)=0 THEN
STEP
ENDIF
Este si da verdadero si se ejecuta.
Se puede añadir un else también ej:

//NOMBRE IF RC=8 THEN
//STEP
//NOMBREELSE ELSE 
//STEP2
//ENDIF

Para crear GDG datasets necesitaremos al final del archivo colocar una G de generacion y luego la V de version. EJ:
Cada uno corresponde a un mes y un año.
BANK.CUSTOMERS.MONTHLY.G0022V01
BANK.CUSTOMERS.MONTHLY.G0022V02
BANK.CUSTOMERS.MONTHLY.G0022V03
Para hacerlos siempre tenemos que usar la misma base osea BANK.CUSTOMERS.MONTHLY. La generacion máxima puede ser 255

LIMIT sirve para limitar la cantidad de versiones por generacion. en este ejemplo podemos hacer LIMIT=12 para los meses
EMPTY borra del catalogo todas las versiones existentes.
NOEMPTY solo borra del catalogo la generacion más vieja cuando se alcanza el limite 
SCRATCH Borra la generacion cuando se descataloga
NOSCRATCH no borra la generacion cuando se descataloga

Para crear un GDG en jcl tenemos que usar el programa IDCAMS EJ:
//STEP01 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN DD *
    DEFINE GDG(NAME(BANK.CUSTOMERS.MONTHLY) -
	   LIMIT(12)			    - 
	   NOEMPTY			    -	
	   NOSCRATCH
/*

Y para agregar una nueva generación:
//STEP01 EXEC PGM=IEFBR14
//DD1 DD DSN=BANK.CUSTOMERS.MONTHLY(+1)
//	 DISP=(NEW,CATALOG,DELETE)
//       SPACE=...
//       DCB=...
//SYSPRINT DD SYSOUT=*
/*

Para referenciar un GDG específico lo debemos hacer con () y un numero. Ej:
BANK.CUSTOMER.MONTHLY(0) O (-1)
0 Se usa para referenciar al ultimo creado y en negativo para alguno más antiguo
También se puede hacer con el nombre completo EJ:
BANK.CUSTOMER.MONTHLY.G00V01

Para borrar un GDG completo debemos :
//SYSIN	DD * 
	DELETE(NOMBRE.GDG) GDG FORCE/PURGE
/*

IEFBR14 Es la utility dummy, se usa para crear datasets o eliminarlos. Ej.
PROG1 EXEC IEFBR14
DD1 DD DISP=(NEW,CATLG,DELETE)
DD1 DD DISP=(OLD,CATLG,DELETE)

IEBCOPY Sirve para copiar un miembro de un PDS a otro PDS
Se puede usar las keywords 
EXCLUDE MEMBERS 
INCLUDE MEMBERS para incluir o excluir 
Ej.
//JOB statement
//STEP1 EXEC PGM-IEBCOPY
//DD1 DD DSN=<Input pds> DISP-SHR
//DD2 DD DSN=<Ouput pds> DISP=SHR
//SYSPRINT DD SYSOUT=*
//SYSOUT DD SYSOUT=* I
/SYSIN DD *
COPY INDD DD1,OUTDD=DD2
SELECT MEMBER (Mem1, Mem8)

IEHLIST Sirve para listar entradas en un DASD VTOC, listar entradas en un PDS directory, listar entradas en un system catalog. Ej

//STEP1 EXEC PGM=IEHLIST
//SYSPRINT DD SYSOUT=*
//DD1 DD DISP=OLD UNIT-SYSDA, VOL=SER=ABC
//SYSIN DD *
	LISTPDS DSNAME=TEST.ACCT PDS

IEHPROGM
Sirve para borrar un dataset, catalogar o descatalogar un dataset, renombrar un dataset. 
Ej.
//STEP1 EXEC PGM=IEHPROGM
//SYSPRINT DD SYSOUT=A
//SYSUTI DD UNIT=3390 VOL-SER=V001
//SYSIN DD UNCATLG DSNAME= TEST ACCT MEMLST VOL-SER=V001,UNIT=SYSDA

IEBEDIT

IEBPTPCH

 IEBGENER sirve para realizar copias de ficheros secuenciales, también es util para editar datsets, imprimir datasets secuenciales o miembros de un pds o crear un miembro de pds desde un dataset secuencial .
 
SYSPRINT Es para mensajes
SYSIN define el control de los statements para el programa
SYSUT define el input
SYSUT2 define el output
EJ: 
//JOB STATEMENT
//STEP1 EXEC PGM=IEBGENER
//SYSUT1 DD DSN=<Input dataset>DISP=SHR
//SYSUT2 DD DSN=<Output dataset>.
//	DISP (CATLG,DELETE),
//	SPACE (TRK,(1,1)),VOL=SER=LP1WK2,
//	RECFM=FB.LRECL=80,UNIT=SYSDA
//SYSIN DD DUMMY
/SYSPRINT DD SYSOUT=*


En sort si queremos que no haya duplicados, ademas de sort fields, usamos SUM FIELDS=NONE
