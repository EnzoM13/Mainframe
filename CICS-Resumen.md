Crear un BMS mapset

Nombremap DFHMSD TYPE=&SYSPARM,MODE=INOUT,TERM=3270,CTRL=FREEKB,       X
		STORAGE=AUTO,LANG=COBOL,TIOAPFX=YES			
nombre.primer.codigo DFHMDI SIZE=(24,80),LINE=1,COLULM=1

		     DFHMDF POS=(1,1),INITIAL="HELLO WORLD",LENGHT=11, x
			    ATTRB=PROT

DFHMSD TYPE=FINAL Define el final del mapa
END

Para compilar un CICS tenemos que hacer un JCL 

//NEW JOB CLASS=A,MSGCLASS=A,MSGLEVEL=(1,1),PRTY=15,NOTIFY=&SYSUID
//PROBLIB JCLLIB ORDER=DFH320.CICS.SDFHPROC
//STEP1 EXEC PROC=DFHMAPS,MAPNAME="nombremapa",
//	     INDEX="DFH320.CICS",
//	     MAPLIB="DFH320.CICS.SDFHLOAD",   Donde se guarda el loadmodude
//	     DSCTLIB="DFH320.CICS.SDFHMAC"    Donde se guarda el copybook
//COPY.SYSUT1 DD DISP=SHR,DSN=nombredataset
//SYSPRINT DD SYSOUT=*
/*





DFHMSD Define el principio del mapa. La X al final significa continuacion 
TERM es la terminal. LANG lenguaje. TIOAPFX Terminal input output area prefix
DFHMDI  Sirve para indicar que comienza un mapa
SIZE=(xx,xx) El primer lugar es para indicar el row y la segunda en la columna
LINE Y COLUMN Sirve para indicar de donde comienzan a escribir 
DFHMDF Sirve para definir un FIELD 
POS=(X,X) Donde empieza
INITIAL= Sirve para escribir 
LENGHT=X Sirve para decir cuanto va a ocupar 
ATTRB=PROT Sirve para no poder sobreescribir

Definir un mapaset:
CEDA DEF MAPSET(NOMBRE) GROUP(NOMBREGRUPO)
Para instalarlo:
CEDA INSTALL MAPSET(NOMBRE) GROUP(NOMBREG)
Send a mapset:
CECI SEND MAP(NOMBRE)
