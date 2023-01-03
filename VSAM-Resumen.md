KSDS Emplea dos subficheros para tratamiento de datos, uno para el almacenamiento de datos y otro para la info asociada a cada indice. Osea en data se guardan por ejemplo los nombres y los numeros de teléfono. En index se guarda la ruta a ellos.

RRDS se puede acceder al record usando un record number 
ESDS Se accede a los records de manera secuencial. 
LDS la data se guarda en byte-streams como en notepad. 
Para definir un VSAM se usa IDCAMS y no se pueden ver en ISPF
Un VSAM está hecho de control intervals(la unidad más chica de transferencia entre el disco  y el OS)
Control interval contiene:
1. records
2. espacio libre
3. CI fields: RDF y CIDF
   RDF describe el largo de los records
   CIDF contiene la informacion sobre el CI 
Se pueden juntar los CI para crear un CONTROL AREAL (CA)
Cada CA tiene un sequence set, guarda el ultimo lugar de los CI dentro del CA, El index set guarda el ultimo lugar del sequence set de cada CA 

Free space es el porcentaje de espacio reservado en un CI y CA
Su sintaxis es: FREESPACE=(%CI to be free, %CA to be free)
Ej: FREESPACE=(20,25) Significa que el 20% de los CI está vacio y el 25% de los CA está vacio 
Cuando se le quiere añadir más data a un CI que ya está lleno se divide, osea se hace un SPLIT y parte de esa info se va a otro CA 


KSDS Key sequence dataset 
Los records son guardados de manera secuencial ascendente por una key
Los records pueden ser ordenados por la Key
Las key son unicas
No existen records duplicados porque la key es unica 
Los parametros requeridos son:
Name: Nombre el VSAM
KEYS: la sintaxis es KEYS(lenght offset) Se pone la cantidad de caracteres que queremos usar como Key y desde donde queremos que empiece EJ: KEYS(5 5)
RECSZ: la sintaxis es RECSZ(average maximum) Se refiere a cuantos caracteres por linea pueden entrar average es un estimativo y maximum la cantidad maxima permitida EJ: RECSZ(90 200)
FREESPACE: La sintaxis es FREESPACE(%CI %CA)
INDEXED: Sirve para indicar que es un KSDS
CISZ: Es el tamaño de cada CI el tamaño estandar es 4096 bytes y tiene un maximo de 32kb
SPACE: Espacio total alocado al VSAM la sintaxis es UNIT(primary secondary) EJ: TRACKS(30 50)
VOLUME: 

Para crear un VSAM KSDS debemos hacer un JCL

//STEP1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN DD *
	DEFINE CLUSTER(         -
	NAME(XXX.XXX.KSDS)      - 
	INDEXED			-
	KEY(5 0)		-
	RECSZ(90 200)		-
	FREESPACE(10 20)	-
	TRACKS(50 30)		-
	CISZ(8192)		-		
	VOLUME(ZASYS1))		
/*

ESDS entry sequence data set 
Los records se guardan de manera secuencial, no se pueden eliminar records. Se pueden crear records duplicados
Los parametros requeridos son:
NAME, RECSZ, FREESPACE, NONINDEXED, CISZ, SPACE, VOLUME. 
 
Para crear un VSAM ESDS debemos hacer un JCL

//STEP1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN DD *
	DEFINE CLUSTER(		-	
	NAME(XXX.XXX.ESDS)	-
	NONINDEXED		-
	RECSZ(90 200)		-
	FREESPACE(10 10)	-
	CISZ(8192)		-
	TRACKS(20 20)		-	
	VOLUME(ZASYS1))
/*		

RRDS relative record dataset
Tiene records que son identificados por su relative record number. Osea el primer record es RR1 el segundo es RR2 etc
Los records nuevos se guardan en un lugar libre, los records pueden ser borrados y se crea un lugar libre al borrarse. 
Los parametros requeridos son:
NAME, RECSZ, FREESPACE, NUMBERED, CISZ, SPACE, VOLUME. 

Para crear un VSAM RRDS debemos hacer un JCL

//STEP1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN DD *
	DEFINE CLUSTER(		-	
	NAME(XXX.XXX.ESDS)	-
	NUMBERED		-
	RECSZ(90 200)		-
	FREESPACE(10 10)	-
	CISZ(8192)		-
	TRACKS(20 20)		-	
	VOLUME(ZASYS1))
/*		
 
LDS es un stream de data, no  hay records solo data bytes 
Los parametros requeridos son:
NAME, LINEAR , CISZ, SPACE, VOLUME. 

Para crear un VSAM LDS debemos hacer un JCL
//STEP1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSIN DD *
	DEFINE CLUSTER(		-	
	NAME(XXX.XXX.ESDS)	-
	LINEAR		-
	CISZ(8192)		-
	TRACKS(20 20)		-	
	VOLUME(ZASYS1))
/*		

DITTO utility sirve para editar data de un VSAM

LISTCAT Sirve para ver las propiedades de un VSAM que no hayamos creado
Se crea un trabajo nuevo en jcl 

//step0 exec pgm=idcams
//sysprint dd sysout=*  (acá se puede copiar la info a un dataset también con disp=shr,dsn=xxx.xx.xxx)
//sysin dd *
	listcat entry(nombre.vsam.ksds) -
	all
/*


REPRO commands sirve para copiar data dentro de un VSAM
De ps a VSAM 

//STEP 1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSOUT DD SYSOUT=*
//PSFILE DD DISP=SHR,DNS=NOMBRE.DATA.SET
//KSDSFILE DD DISP=SHR,DSN=NOMBRE.KSDS (SE PUEDE HACER CON CUALQUIER ARCHVIO VSAM)
//SYSIN DD *
	REPRO INFILE(PSFILE) -
	OUTFILE(KSDSFILE)
/*

VSAM A VSAM

//STEP 1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSOUT DD SYSOUT=*
//PSFILE DD DISP=SHR,DNS=NOMBRE.DATA.SET
//SYSIN DD *
	REPRO INFILE(PSFILE) -
	OUTFILE(NOMBNRE.SALIENTE.KSDS)
/*


Para añadir otro index a un VSAM (alternative index) debemos primero crear un jcl indicando la nueva key

//STEP1 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSOUT DD SYSOUT=*
//SYSIN DD *
	DEFINE AIX(NAME(NOMBRE.DEL.KSDS.NOMBREAIX) -
	RELATE(NOMBRE.DEL.KSDS) -
	KEYS(X X) - 
	RECSZ(X X) - 	
	FREESPACE(X X) -
	TRACKS(X X)	-
	CISZ(X)	-
	VOLUME(X)	-
	UNIQUEKEY/NONUNIQUEKEY (SIRVE PARA INDICAR SI EL VALOR DE LA KEY ES UNICO O NO)
	UPGRADE/NONUPGRADE(SIRVE PARA INDICAR SI QUEREMOS QUE LOS DOS SE ACTUALICEN CUANDO SE AÑADA UN NUEVO VALOR AL KSDS VIEJO
/*

Para definir un path del aix debemos:

//STEP2 EXEC PGM=IDCAMS
//SYSPRINT DD SYSOUT=*
//SYSOUT DD SYSOUT=*
//SYSIN DD *
	DEFINE PATH(NAME(NOMBRE.DEL.KSDS.NOMBREAIX.PATH) -
	PATHENTRY(NOMBRE.DEL.KSDS.NOMBREAIX)		-
/*
Se puede hacer en la misma job card que la creación del AIX

Para contruir el nuevo index debemos: (sirve para poner la data en el AIX)
//STEP3 PGM=IDCAMS
//SYSOUT 
//SYSPRINT 
//SYSIN DD *
	BLDINDEX	-	
	INDATASET(NOMBRE.DEL.KSDS)	-	
	OUTDATASET(NOMBRE.DEL.KSDS.NOMBREAIX)	-
/*
	 
