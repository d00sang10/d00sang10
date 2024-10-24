- 👋 Hi, I’m @d00sang10
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
d00sang10/d00sang10 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
------------------------------------------------------------------------------------------------------------
----BASE DE DATOS AVANZADOS SEMANA1 Y 2
/*
TEMARIO
1. Fundamentos de Programación TRANSACT-SQL
2. Construcciones de Programación TRANSACT-SQL
2.1. Variables
2.1.1. Variables Locales
2.1.2. Variables Públicas
2.2. Herramientas para el control de flujos
2.2.1. Estructuras de control IF
2.2.2. Estructura condicional CASE
2.2.2.1. Usar una instrucción SELECT con una expresión CASE de búsqueda
2.2.3. Estructura de control WHILE
2.3. Control de errores en TRANSACT-SQL
2.3.1. Funciones especiales de ERROR
2.3.2. Variable de sistema @@ERROR
2.3.3. Generar un error RAISERROR
2.4. Cursores en TRANSACT-SQL
2.4.1. DECLARE Cursor
2.4.2. Abrir un Cursor: OPEN
2.4.3. Leer un Registro: FETCH
2.4.4. Cerrar el Cursor: CLOSE
2.4.5. Liberar los recursos: DEALLOCATE
-------------------------------------------
CONSTRUCCION DE PROGRAMACION TRANSACT-SQL
TRANSACT-SQL amplía el SQL estándar con la implementación de estructuras de
programación. Estas implementaciones le resultarán familiares a los desarrolladores
con experiencia en C++, Java, Visual Basic .NET, C# y lenguajes similares. A
continuación, vamos a definir las estructuras de programación implementadas en SQL
Server.
2.1.2 VARIABLES
Una variable es una entidad a la que se asigna un valor. Este valor puede cambiar
durante el proceso donde se utiliza la variable. SQL Server tiene dos tipos de
variables: locales y globales. Las variables locales están definidas por el usuario,
mientras que las variables globales las suministra el sistema y están predefinidas.
-----Variables Locales
Las variables locales se declaran, nombran y escriben mediante la palabra clave
declare, y reciben un valor inicial mediante una instrucción select o set
Los nombres de las variables locales deben empezar con el símbolo “@”. A cada
variable local se le debe asignar un tipo de dato definido por el usuario o un tipo de
dato suministrado por el sistema distinto de text, image o sysname. 
Sintaxis
	DECLARA UNA VARIABLE
		 DECLARE @VARIABLE <TIPO DE DATO>
		 ASIGNA VALOR A UNA VARIABLE
		 SET @VARIABLE= VALOR

*/
-- VARIABLES LOCALES Y GLOBALES
USE neptuno
--1
DECLARE @PRECIO DECIMAL
SET @PRECIO = 50
SELECT * FROM PRODUCTOS
WHERE PRECIOUNIDAD >	@PRECIO
GO
--
--Podemos utilizar la instrucción SELECT en lugar de la instrucción SET. Una
--instrucción SELECT utilizada para asignar valores a una o más variables se denomina
--SELECT de asignación. Si utilizamos el SELECT de asignación, no puede devolver
--valores al cliente como un conjunto de resultados.
--En el ejemplo siguiente, declaramos dos variables y le asignamos el máximo y mínimo
--precio desde la tabla Compras.productos.

-- bloques anonimos
SELECT * FROM PRODUCTOS
---2
DECLARE @MX DECIMAL, @MN DECIMAL 
SELECT	@MX=MAX(PRECIOUNIDAD),
@MN=MIN(PRECIOUNIDAD)  FROM PRODUCTOS
-- imprimir los variables
PRINT 'MAYOR PRECIO:'+STR(@MX) 
PRINT 'MENOR PRECIO:'+STR(@MN)  
--
--- VARIABLES GLOBALES

-------------
---Variables Públicas
--Las variables globales son variables predefinidas suministradas por el sistema. Se
--distinguen de las variables locales por tener dos símbolos “@”. Estas son algunas
--variables globales del servidor:
-------------
/*Variable		Contenido
@@ERROR			Contiene 0 si la última transacción se ejecutó de forma
				correcta; en caso contrario, contiene el último número de
				error generado por el sistema. La variable global @@error
				se utiliza generalmente para verificar el estado de error de
				un proceso ejecutado.
@@IDENTITY		Contiene el último valor insertado en una columna
				IDENTITY mediante una instrucción insert
@@VERSION		Devuelve la Versión del SQL Server
@@SERVERNAME	Devuelve el Nombre del Servidor
@@LANGUAGE		Devuelve el nombre del idioma en uso
@@MAX_CONNECTIONS Retorna la cantidad máxima de conexiones permitidas
*/
-----------
use master
--LA VERSION DEL SQL SERVER
PRINT 'VERSION:' + @@VERSION

--LENGUAJE DEL APLICATIVO 
PRINT 'LENGUAJE:' + @@LANGUAGE

--NOMBRE DEL SERVIDOR
PRINT 'SERVIDOR:' + @@SERVERNAME

----NUMERO DE CONEXIONES PERMITIDAS
PRINT 'CONEXIONES:' + STR(@@MAX_CONNECTIONS)
--
/*
HERRAMIENTAS PARA EL CONTROL DE FLUJOS
El lenguaje de control de flujo se puede utilizar con instrucciones interactivas, en lotes
y en procedimientos almacenados. El control de flujo y las palabras clave relacionadas
y sus funciones son las siguientes:
-----------------------------------
 Palabra Clave Función
IF … ELSE		Define una ejecución condicional, cuando la condición la
				condición es verdadera y la alternativa (else) cuando la
				condición es falsa
CASE			Es la forma más sencilla de realizar operaciones de tipo IFELSE
				IF-ELSE IF-ELSE. La estructura CASE permite evaluar
				una expresión y devolver un valor alternativo
WHILE			Estructura repetitiva que ejecuta un bloque de instrucciones
				mientras la condición es verdadera
BEGIN … END		Define un bloque de instrucciones. El uso del BEGIN…END
				permite ejecutar un bloque o conjunto de instrucciones.
DECLARE			Declara variables locales
BREAK			Sale del final del siguiente bucle while más interno
…CONTINUE		Reinicia del bucle while
RETURN [n]		Sale de forma incondicional, suele utilizarse en
				procedimientos almacenados o desencadenantes.
				Opcionalmente, se puede definir un numero entero como
				estado devuelto, que puede asignarse al ejecutar el
				procedimiento almacenado
PRINT			Imprime un mensaje definido por el usuario o una variable
				local en la pantalla del usuario
/*COMENTARIO*/ Inserta un comentario en cualquier punto de una instrucción
				SQL
--COMENTARIO	Inserta una línea de comentario en cualquier punto de una
				instrucción SQL
SINTAXIS
	IF (<expression>)
		BEGIN
		...
		END
	ELSE IF (<expression>)
		BEGIN
		...
		END
	ELSE
		BEGIN
		...
		END


*/




--Ejemplo: Visualice un mensaje donde indique si 
--un empleado (ingrese su codigo) ha  realizado pedidos.
use neptuno
SELECT * FROM PEDIDOS
----->
DECLARE @IDEMP INT, @CANTIDAD INT
SET @IDEMP = 1--0
--RECUPERAR LA CANTIDAD DE PEDIDOS DEL EMPLEADO DE CODIGO ?? 
SELECT @CANTIDAD = COUNT(*)
FROM PEDIDOS WHERE IDEMPLEADO = @IDEMP
--EVALUA EL VALOR DE CANTIDAD 
IF	@CANTIDAD = 0
PRINT 'EL EMPLEADO NO HA REALIZADO ALGUN PEDIDO' 
	ELSE IF @CANTIDAD = 1
PRINT 'HA REGISTRADO 1 PEDIDO, CONTINUE TRABAJANDO'
ELSE
PRINT 'HA REGISTRADO PEDIDOS'
--   <---- GO
SELECT IDEMPLEADO, COUNT(*) PEDIDOS_EJECUTADOS FROM PEDIDOS
GROUP BY IDEMPLEADO
GO
---Ejemplo: utilizamos la estructura IF para evaluar la existencia de un registro; si existe
--actualizamos los datos de la tabla; si no existe (ELSE) insertamos el registro
-- USAR BD DBRESERVA

USE DBRESERVA 
-- EVITA MODIFICAR LA DAT CON BEGIN TRANSACTION/COMMIT / ROLLBACK
--
SELECT * FROM PAIS order by 1
BEGIN TRANSACTION
--
DECLARE @IDPAIS CHAR(4), @NOMBRE VARCHAR(50)
SET @IDPAIS = '0014'
SET @NOMBRE = 'Africa'
--EVALUA SI EXISTE EL REGISTRO DE LA TABLA, SI EXISTE  ACTUALIZO, SINO INSERTO
IF EXISTS(SELECT * FROM PAIS WHERE IDPAIS = @IDPAIS) 
	BEGIN
		UPDATE PAIS
		SET NOMBRE = @NOMBRE	
			WHERE IDPAIS = @IDPAIS
	END		
ELSE
	BEGIN
	INSERT INTO PAIS VALUES (@IDPAIS, @NOMBRE)
	END
GO
--
rollback
SELECT * FROM PAIS
--
/*
Estructura condicional CASE
La estructura CASE evalúa una lista de condiciones y devuelve una de las varias
expresiones de resultado posibles. La expresión CASE tiene dos formatos:
• La expresión CASE sencilla compara una expresión con un conjunto de
expresiones sencillas para determinar el resultado.
• La expresión CASE buscada evalúa un conjunto de expresiones booleanas
para determinar el resultado.
Ambos formatos admiten un argumento ELSE opcional. La sintaxis del CASE

CASE <expresión>
		WHEN <valor_expresion> THEN <valor_devuelto>
		WHEN <valor_expresion1> THEN <valor_devuelto1>
		ELSE <valor_devuelto2> -- Valor por defecto
END

Ejemplo: Declare una variable donde le asigne el numero del mes, evalúe el valor de
la variable y retorne el mes en letras.

*/

DECLARE @M INT, @MES VARCHAR(20) 
SET @M=5
SET @MES = (CASE @M
		WHEN 1	THEN 'ENERO'
		WHEN 2	THEN 'FEBRERO'
		WHEN 3	THEN 'MARZO'
		WHEN 4	THEN 'ABRIL'
		WHEN 5	THEN 'MAYO'
		WHEN 6	THEN 'JUNIO'
		WHEN 7	THEN 'AGOSTO'
		WHEN 8	THEN 'AGOSTO'
		WHEN 9	THEN 'SETIEMBRE'
		WHEN 10	THEN 'OCTUBRE'
		WHEN 11	THEN 'NOVIEMBRE'
		WHEN 12	THEN 'DICIEMBRE'
	ELSE 'NO ES VALIDO'
END)
PRINT @MES
----------------------------------
---------------------------------
use neptuno
select * from Empleados

--Ejemplo: Mostrar los datos de los empleados evaluando el valor del campo
--tratamiento asignando, para cada valor, una expresión

SELECT (CASE TRATAMIENTO
		WHEN 'SRTA.' THEN 'SEÑORITA'
		WHEN 'SR.' THEN 'SEÑOR'
		WHEN 'DR.' THEN 'DOCTOR'
		WHEN 'SRA.' THEN 'SEÑORA'
		ELSE 'NO TRATAMIENTO'
		END) ,APELLIDOS, NOMBRE
FROM EMPLEADOS
ORDER BY 1;
GO
-----------------------------
-- NEPTUNO
USE NEPTUNO
SELECT * FROM productos
--
--En el ejemplo siguiente, listamos los datos de los productos y definimos una columna
--llamada ESTADO, el cual evaluará stock de cada producto imprimiendo un valor:
--Stockeado, Limite, Haga una solicitud.
DECLARE @STOCK INT
SET @STOCK=20

SELECT NOMBREPRODUCTO, PRECIOUNIDAD,  UNIDADESENEXISTENCIA, 
'ESTADO'=	(CASE WHEN UNIDADESENEXISTENCIA>@STOCK 	THEN 'STOCKEADO' 
				  WHEN UNIDADESENEXISTENCIA=@STOCK THEN 'LIMITE' 
				  WHEN UNIDADESENEXISTENCIA<@STOCK THEN 'HAGA UNA SOLICITUD'
			END)
FROM PRODUCTOS 
GO
------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------
--MAS EJEMPLOS CASE
select DAY(getdate())
select MONTH(getdate())
-- mostrar la fecha en texto registrada en la tabla reserva
USE DBRESERVA
select * from reserva
select *, 
cast (day(fecha) as char(2))+
case month(fecha)
	when 1 then ' Enero '
	when 2 then ' Febrero '
	when 3 then ' Marzo '
	when 4 then ' Abril '
	when 5 then ' Mayo '
	when 6 then ' Junio '
	when 7 then ' Julio '
	when 8 then ' Agosto '
	when 9 then ' Setiembre '
	when 10 then ' Octubre '
	when 11 then ' Noviembre '
	when 12 then ' Diciembre '
end
+ cast (year(fecha) as char(4))as [Fecha Texto]
	from RESERVA
	---- LO MISMO PERO EL LA BD NEPTUNO
	USE neptuno
	select IdEmpleado,APELLIDOS, FECHANACIMIENTO, 
cast (day(FechaNacimiento) as char(2))+
case month(FechaNacimiento)
	when 1 then ' Enero '
	when 2 then ' Febrero '
	when 3 then ' Marzo '
	when 4 then ' Abril '
	when 5 then ' Mayo '
	when 6 then ' Junio '
	when 7 then ' Julio '
	when 8 then ' Agosto '
	when 9 then ' Setiembre '
	when 10 then ' Octubre '
	when 11 then ' Noviembre '
	when 12 then ' Diciembre '
end
+ cast (year(FechaNacimiento) as char(4))as [Fecha Texto]
	from Empleados


----------------------------------------------------------------------------------------------------------------------------------

--- WHILE
--Estructura de control WHILE
--La estructura WHILE ejecuta en forma repetitiva un conjunto o bloque de instrucciones
--SQL siempre que la condición especificada sea verdadera. Se puede controlar la
-- de instrucciones en el bucle WHILE con las palabras clave BREAK y
--CONTINUE.
--La sintaxis de WHILE es:
		WHILE <expresion>
				BEGIN
				...
		END
--Por ejemplo: Implemente un programa que permita listar los 100 primeros números
--enteros, visualizando en cada caso si es par o impar.


DECLARE @contador int
SET @contador = 0 
WHILE (@contador < 100)	
		BEGIN
			SET @contador = @contador + 1  
				IF	@contador %2 =0
				PRINT cast(@contador AS varchar) +' es un Número Par'
				ELSE
				PRINT cast(@contador AS varchar) + ' es un Número Impar'
		END

--Listar los 5 primeros registros de la tabla productos
select * from productos  
select  * from productos where idproducto=5
select * from clientes where idCliente= 'anton'

DECLARE @COUNTER INT = 1;
WHILE @COUNTER < 6  
		BEGIN
			SELECT TOP(1) IDPRODUCTO, NOMBREPRODUCTO, PRECIOUNIDAD  FROM PRODUCTOS 
			WHERE IDPRODUCTO = @COUNTER  
					SET @COUNTER += 1;
		END;

-- BREAK, CONTINUE
--BREAK y CONTINUE controlan el funcionamiento de las instrucciones dentro de un
--bucle WHILE. BREAK permite salir del bucle WHILE. CONTINUE hace que el bucle
--WHILE se inicie de nuevo. La sintaxis de BREAK y CONTINUE es:
*/--
WHILE BOOLEAN_EXPRESION
	BEGIN
		EXPRESION_SQL

		[BREAK]
			[EXPRESION_SQL]

		[CONTINUE]
			[EXPRESION_SQL]
	END

/*
*/
--- Actualizar las unidades de existencia de los productos asignándoles el  valor de 1000
-- de aquellos productos cuyo stock sea cero. BD NEPTUNO
--
USE NEPTUNO

SELECT IDPRODUCTO,unidadesEnExistencia FROM productos WHERE unidadesEnExistencia= 0
--
begin transaction

DECLARE @ID INT, @NOMBRE VARCHAR(50)  
	WHILE	EXISTS (SELECT * FROM PRODUCTOS WHERE UNIDADESENEXISTENCIA=0)
			BEGIN
				SELECT TOP 1 @ID= IDPRODUCTO, @NOMBRE=NOMBREPRODUCTO 
						FROM PRODUCTOS 	WHERE UNIDADESENEXISTENCIA=0

			UPDATE PRODUCTOS
					SET UNIDADESENEXISTENCIA=1000 
					WHERE IDPRODUCTO=@ID

			PRINT 'PRODUCTO:'+@NOMBRE + ' SE ACTUALIZO EL STOCK' 
			CONTINUE
	END;
			select * from productos
rollback transaction

-- ERRORES 
--TRY Y CATCH
-----
/*
CONTROL DE ERRORES EN TRANSACT-SQL

SQL Server proporciona el control de errores a través de las instrucciones TRY y  CATCH.
Estas nuevas instrucciones suponen un gran paso adelante en el control de  errores en SQL Server.
La sintaxis de TRY CATCH es la siguiente

*/

	BEGIN TRY
		EXPRESION_SQL  
	END TRY
	BEGIN CATCH
		EXPRESION_SQL 
	END CATCH

/*
Implemente un programa que evalúa la división de dos números enteros.
Si la  división ha sido exitosa, imprima un mensaje: NO HAY ERROR.
Caso contrario  imprimir un mensaje: SE HA PRODUCIDO UN ERROR

*/
	BEGIN TRY
		DECLARE @DIVISOR INT, @DIVIDENDO INT, @RESULTADO INT  
		SET @DIVIDENDO = 100
		SET @DIVISOR = 2   -- 0
-- ESTA LINEA PROVOCA UN ERROR DE DIVISION POR 0
		SET @RESULTADO = @DIVIDENDO/@DIVISOR 
		PRINT 'NO HAY ERROR'
	END TRY
	BEGIN CATCH
	PRINT 'SE HA PRODUCIDO UN ERROR'  
	END CATCH;
	-------------------------------------------------------------------
	--Funciones especiales de Error

--Las funciones especiales de error, están disponibles únicamente en el  bloque CATCH 
--para la obtención de información detallada del error. 
--A continuación,  presentamos las funciones que se utilizan en el control de errores
--   ERROR                  DESCRIPCION
-- ERROR_NUMBER()		 Devuelve el numero de error
-- ERROR_SEVERITY()		 Devuelve la severidad del error
-- ERROR_STATE()		 Devuelve el estado del error
-- ERROR_PROCEDURE()	 Devuelve el nombre del procedimiento almacenado
--						 que ha provocado el error
-- ERROR_LINE()			 Devuelve el número de línea en la que se ha
--						 producido el error.
--ERROR_MESSAGE()		 Devuelve el mensaje de error
--Ejemplo: Implemente un programa que evalúa la división de dos números enteros. 
--Si  la división ha sido exitosa, imprima un mensaje: NO HAY ERROR. 
--Caso contrario  imprimir el mensaje de error y el estado del error

	--
	BEGIN TRY
		DECLARE @DIVISOR INT, @DIVIDENDO INT, @RESULTADO INT 
		SET @DIVIDENDO = 100
		SET @DIVISOR = 0
-- ESTA LINEA PROVOCA UN ERROR DE DIVISION POR 0 
		SET @RESULTADO = @DIVIDENDO/@DIVISOR
		PRINT 'NO HAY ERROR'  
		END TRY
	BEGIN CATCH
	PRINT ERROR_MESSAGE()  
	PRINT ERROR_STATE()
	PRINT ERROR_LINE()
	END CATCH;
	--
	--@@ERROR
	--Implemente un bloque de instrucciones que permita eliminar un registro de  Cliente por su código.
	--Si hay un error en el proceso, visualice un mensaje de error
	BEGIN TRANSACTION
	SELECT * FROM clientes
	--
	DELETE FROM CLIENTES WHERE IDCLIENTE = 'ALFKI'
IF @@ERROR<>0
PRINT 'NO SE PUEDE ELIMINAR'
--
use DBRESERVA
select * from PAIS
--
delete from PAIS where idpais='0012'

DELETE FROM pais WHERE IDpais = '0012'
IF @@ERROR<>0
PRINT 'NO SE PUEDE ELIMINAR'
--
ROLLBACK

---
use neptuno
	BEGIN TRY
		DELETE FROM CLIENTES  WHERE IDCLIENTE = 'ALFKI'
	END TRY
	BEGIN CATCH
		IF @@ERROR=547
		PRINT 'NO SE PUEDE ELIMINAR ESTE CLIENTE' 
	  PRINT ERROR_MESSAGE()  
	--  PRINT ERROR_STATE()
	--  PRINT ERROR_LINE()
	--  PRINT ERROR_STATE()
	
	END CATCH

---------------------------

--La variable @@ERROR devuelve un número de error que representa el error de la
--operación. Si el error se encuentra en la vista de catálogo sys.sysmessages,
--entonces @@ERROR, contendrá el valor de la columna sys.sysmessages.error para
--dicho error. Puede ver el texto asociado con el número de error @@ERROR en
--sys.sysmessages.description

---Ejemplo: Implemente un proceso que elimine los productos cuyo valor de sus unidades
--en existencia, sea menor a 20, en caso de no ejecutar exitosamente el error,
--implemente el CATCH para controlar el error.
		USE neptuno
		BEGIN TRANSACTION
		select * from productos

		BEGIN TRY
		DELETE FROM PRODUCTOS
		WHERE UNIDADESENEXISTENCIA<20
		END TRY
		BEGIN CATCH
		DECLARE @MENSAJE VARCHAR(255)
		--RECUPERAR LA DESCRIPCION DEL VALOR DE @@ERROR
		SELECT @MENSAJE= M.DESCRIPTION
		FROM SYS.SYSMESSAGES M WHERE M.ERROR=@@ERROR
		PRINT @MENSAJE
		END CATCH

/*
Generar un error RAISERROR
En ocasiones, es necesario provocar voluntariamente un error, por ejemplo nos puede
interesar que se genere un error cuando los datos incumplen una regla de negocio.
Podemos provocar un error en tiempo de ejecución a través de la función
RAISERROR.
La función RAISERROR recibe tres parámetros, el mensaje del error (o código de
error predefinido), la severidad y el estado.
La severidad indica el grado de criticidad del error. Admite valores de 0 al 25, pero
solo podemos asignar valores del 0 al 18. Los errores el 20 al 25 son considerados
fatales por el sistema, y cerraran la conexión que ejecuta el comando RAISERROR.
Para asignar valores del 19 al 25, necesitaremos ser miembros de la función de SQL
Server sysadmin.
En el presente ejercicio, evaluamos los valores de dos variables provocando un error
utilizando RAISERROR.

*/
---------------------------
	DECLARE @TIPO INT, @CLASIFICACION INT
		SET @TIPO = 1
		SET @CLASIFICACION = 3
		IF (@TIPO = 1 AND @CLASIFICACION = 3) 
			BEGIN
				RAISERROR ('EL TIPO NO PUEDE VALER UNO Y LA CLASIFICACION 3',  16, -- SEVERIDAD
																				1	-- ESTADO
			)
		END
------------------------------------------------
select * from productos
select * from categorias
INSERT productos VALUES(78,'Inka Kola',12,9,'24 botellas',3.5,50,0,0,0,'')


---
BEGIN TRY
	SET NOCOUNT ON
INSERT productos VALUES(78,'Inka Kola',12,9,'24 botellas',3.5,50,0,0,0,'')
END TRY
BEGIN CATCH
	RAISERROR('Error al ingresar un producto', 16 ,1)
END CATCH



------------------------------------------------

/*
CURSORES EN TRANSACT-SQL
Un cursor es una variable que nos permite recorres con un conjunto de resultados
obtenidos a través de una sentencia SELECT fila por fila.
El uso de los cursores es una técnica que permite tratar fila por fila el resultado de una
consulta, contrariamente al SELECT SQL que trata a un conjunto de fila. Los cursores
pueden ser implementador por instrucciones TRANSACT-SQL (cursores ANSI-SQL) o
por la API OLE-DB.
Se utilizarán los cursores ANSI cuando sea necesario tratar las filas de manera
individual en un conjunto o cuando SQL no pueda actuar únicamente sobre las filas
afectadas. Los cursores API serán utilizados por las aplicaciones cliente para tratar
volúmenes importantes o para gestionar varios conjuntos de resultados.
	Cuando trabajemos con cursores, debemos seguir los siguientes pasos:
• Declarar el cursor, utilizando DECLARE
• Abrir el cursor, utilizando OPEN
• Leer los datos del cursor, utilizando FETCH ... INTO
• Cerrar el cursor, utilizando CLOSE
• Liberar el cursor, utilizando DEALLOCATE
	La sintaxis general para trabajar con un cursor es la siguiente

	-- DECLARACIÓN DEL CURSOR
DECLARE <NOMBRE_CURSOR> CURSOR
FOR <SENTENCIA_SQL>
-- APERTURA DEL CURSOR
OPEN <NOMBRE_CURSOR>
-- LECTURA DE LA PRIMERA FILA DEL CURSOR
FETCH <NOMBRE_CURSOR> INTO <LISTA_VARIABLES>
WHILE (@@FETCH_STATUS = 0)
BEGIN
-- LECTURA DE LA SIGUIENTE FILA DE UN CURSOR
FETCH <NOMBRE_CURSOR> INTO <LISTA_VARIABLES>
...
END -- FIN DEL BUCLE WHILE
-- CIERRA EL CURSOR
CLOSE <NOMBRE_CURSOR>
-- LIBERA LOS RECURSOS DEL CURSOR
DEALLOCATE <NOMBRE_CURSOR>

Declare CURSOR
Esta instrucción permite la declaración y la descripción del cursor ANSI.
Sintaxis:
-- DECLARACIÓN DEL CURSOR
DECLARE <NOMBRE_CURSOR> [INSENSITIVE][SCROLL] CURSOR
FOR <SENTENCIA_SQL>
FOR [READ ONLY | UPDATE[ OF LISTA DE COLUMNAS]]
• INSENSITIVE solo se permiten las operaciones sobre la fila siguiente
• SCROLL los desplazamientos en las filas del cursor podrán hacerse en todos
los sentidos.
• UPDATE especifica que las actualizaciones se harán sobre la tabla de origen
del cursor. Un cursor INSENSITIVE con una cláusula ORDER BY no puede
actualizarse. Un cursor con ORDER BY, UNION, DISTINCT o HAVING es
INSENSITIVE y READ ONLY.
Además de esta sintaxis conforme con la norma ISO, TRANSACT-SQL propone una
sintaxis ampliada en la definición de los cursores:
DECLARE <nombre_cursor> [LOCAL | GLOBAL ]
FORWARD_ONLY | SCROLL ]
[STATIC | KEYSET | DYNAMIC | FAST_FOWARD ]
[READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]
[TYPE_WARNING]
FOR <sentencia_sql>
{FOR UPDATE [ OF lista de columnas ]}
LOCAL el alcance del cursor es local en el lote, el procedimiento o la función
en curso de ejecución en el que está definido el curso.
• GLOBAL el alcance del cursor el global a la conexión. La opción de base de
datos default to local cursor está definido en falso de manera
predeterminada.
• FOWARD_ONLY los datos se extraen del cursor por orden de aparición (del
primero al último).
• STATIC se realiza una copia temporal de los datos en la base de datos
tempdb para que el cursor no se vea afectado por las modificaciones que
puedan realizarse sobre la base de datos.
• KEYSET las filas y su orden en el cursor se fijan en el momento de la apertura
del cursor. Las referencias hacia cada una de estas filas de información se
conservan en una tabla temporal en tempdb.
• DYNAMIC el cursor refleja exactamente los datos presentes en la base de
datos. Esto significa que el número de filas, su orden y su valor pueden variar
de forma dinámica.
FAST_FORWARD permite definir un cursor hacia adelante y como sólo lectura
(FORWARD_ONLY y READ_ONLY).
• SCROLL_LOCKS permite garantizar el éxito de las instrucciones UPDATE y
DELETE que pueden ser ejecutadas en relación al cursor. Con este tipo de
cursor, se fija un bloqueo en la apertura para evitar que otra transacción trate
de modificar los datos.
• OPTIMISTIC con esta opción, puede que una operación de UPDATE o DLETE
realizada en el cursor no pueda ejecutarse correctamente, porque otra
transacción haya modificado los datos en paralelo.
• TYPE_WARNING se envía un mensaje (warning) a la aplicación cliente, si se
efectúan conversiones implícitas de tipo.

Abrir un Cursor
---------------
Esta instrucción permite hacer operativo el cursor y crear eventualmente las tablas
temporales asociadas. La variable @@CURSOR_ROWS se asigna después de
OPEN.
Teniendo en cuenta el espacio en disco y la memoria utilizada, y el bloqueo eventual
de los datos en la apertura del cursor, esta operación debe ser ejecutada lo más cerca
posible del tratamiento de los datos extraídos del cursor.
Sintaxis:
OPEN [ GLOBAL ] <nombre_cursor>
Leer un Registro: FETCH
Es la instrucción que permite extraer una fila del cursor y asignar valores a variables
con su contenido. Tras FETCH, la variable @@FETCH_STATUS está a 0 si FETCH
no retorna errores.
Sintaxis:

	FETCH [NEXT | PRIOR | LAST | ABSOLUTE n | RELATIVE n ]
			[FROM] [GLOBAL ] <nombre del cursor>
			[INTO lista de variables ]
	NEXT

. NEXT	lee la fila siguiente (única opción posible para los INSENSITIVE
		CURSOR).
• PRIOR lee la fila anterior
• FIRST lee la primera fila
• LAST lee la última fila
• ABSOLUTE n lee la enésima fila del conjunto
• RELATIVE n lee la enésima fila a partir de la fila actual.
Cuando trabajamos con cursores, la función @@FETCH_STATUS nos indica el
estado de la última instrucción FETCH emitida. Los valores posibles son los
siguientes:

	Valor devuelto				 Descripción
		0				La instrucción FETCH se ejecutó correctamente
		-1				La instrucción FETCH no se ejecutó correctamente o la fila
						estaba más allá del conjunto de resultados.
		-2				Falta la fila recuperada.

Cerrar el cursor
----------------
Cierra el cursor y libera la memoria. Esta operación debe interponerse lo antes posible
con el fin de liberar los recursos cuanto antes. La sintaxis es la siguiente:
					CLOSE <nombre_cursor>
Después de cerrado el cursor, ya no es posible capturarlo o actualizarlo/eliminarlo. Al
cerrar el cursor se elimina su conjunto de claves dejando la definición del cursor
intacta, es decir, un cursor cerrado puede volver a abrirse sin tener que volver a
declararlo.

Liberar los recursos
--------------------

Es un comando de limpieza, no forma parte de la especificación ANSI. La sintaxis es la
siguiente:
		DEALLOCATE <nombre_cursor>
*/
--CURSORES
-- CURSORES------------------------------
USE neptuno
SELECT  * FROM CLIENTES     
SELECT TOP(3)  * FROM clientes
-- CURSORES
-- DECLARANDO UN CURSOR							
DECLARE CURSOR1 CURSOR
	FOR SELECT * FROM clientes
-- ABRIR CURSOR
OPEN CURSOR1
-- NAVEGAR
FETCH NEXT FROM CURSOR1
--CERRAR CURSOR
CLOSE CURSOR1
--LIBERAR CURSOR DE MEMORIA
DEALLOCATE CURSOR1
--EJEMPLO2
-- local el cursor funcionara dentro del prcedimeto almac o triggers dode se a creado
-- global, si quiero q sea visto mas haya
Declare @CODIGO VARCHAR(5),
@COMPAÑIA VARCHAR(200),
@CONTACTO VARCHAR(150),
@PAIS VARCHAR(100)
DECLARE CCLIENTES CURSOR  
	FOR SELECT idCliente,NombreCompañia,NombreContacto,Pais
		FROM clientes
	OPEN CCLIENTES
	FETCH CCLIENTES INTO @CODIGO, @COMPAÑIA,@CONTACTO, @PAIS 
	WHILE (@@FETCH_STATUS=0) -- 0 CORRECTAMENTE, -1 SI NO SE EJECTUTO CORRECTAMENTE, -2 SI HUBO ERROR EN LA FILA RECUPERADA
	BEGIN
	-- PARA IR MOSTRANDO EN PANTALLA LOS VALORES
		PRINT @CODIGO + ' '+ @COMPAÑIA + ' '+ @CONTACTO + ' '+ @PAIS
		FETCH CCLIENTES INTO @CODIGO, @COMPAÑIA,@CONTACTO, @PAIS
		

	END
	CLOSE CCLIENTES
	DEALLOCATE CCLIENTES
	-- el curosr del ejempo 1 solo avanza
--------------------------------------------------------------------------continua  martes 23_02_2021-------------------------------------------------------------------
-- DECLARANDO UN CURSOR
DECLARE CURSOR1 CURSOR scroll
	FOR SELECT * FROM clientes
-- ABRIR CURSOR
OPEN CURSOR1
-- NAVEGAR
FETCH NEXT FROM CURSOR1
FETCH PRIOR FROM CURSOR1
FETCH LAST FROM CURSOR1   --prior regresar,first primero,,last  ultimo,necesito agregar el scroll para q me permita navegar
FETCH FIRST FROM CURSOR1
FETCH ABSOLUTE 10 FROM CURSOR1
FETCH RELATIVE 2 FROM CURSOR1

--CERRAR CURSOR
CLOSE CURSOR1
--LIBERAR CURSOR DE MEMORIA
DEALLOCATE CURSOR1
--------------------
SELECT * FROM clientes
--Ejemplo : Trabajando con un cursor dinámico, defina un cursor dinámico que permita
--visualizar: el primer registro, el registro en la posición 6 y el último registro
DECLARE MI_CURSOR CURSOR SCROLL
FOR SELECT * FROM PRODUCTOS
-- ABRIR
OPEN MI_CURSOR
-- IMPRIMIR LOS REGISTROS
FETCH FIRST FROM MI_CURSOR
FETCH ABSOLUTE 6 FROM MI_CURSOR
FETCH LAST FROM MI_CURSOR
-- CERRAR
CLOSE MI_CURSOR
-- LIBERAR
DEALLOCATE MI_CURSOR

-- LOS CURSORES TAMBIEN NOS PERMITEN MODIFICAR DATOS, COPIAMOS EL EJEMPLO2
--EJEMPLO 3 ACTUALIZAR DATOS clientes
select * from clientes
begin transaction
Declare @CODIGO VARCHAR(5),
@COMPAÑIA VARCHAR(200),
@CONTACTO VARCHAR(150),
@PAIS VARCHAR(100)
DECLARE CCLIENTES CURSOR GLOBAL
	FOR SELECT idCliente,NombreCompañia,NombreContacto,Pais
		FROM clientes FOR UPDATE
	OPEN CCLIENTES
	FETCH CCLIENTES INTO @CODIGO, @COMPAÑIA,@CONTACTO, @PAIS 
	WHILE (@@FETCH_STATUS=0) -- 0 CORRECTAMENTE, -1 SI NO SE EJECTUTO CORRECTAMENTE, -2 SI HUBO ERROR EN LA FILA RECUPERADA
	BEGIN
	-- PARA IR MOSTRANDO EN PANTALLA LOS VALORES
		UPDATE CLIENTES	
			SET NombreCompañia=@COMPAÑIA + '-MODIFICADO'
				WHERE CURRENT OF CCLIENTES
		FETCH CCLIENTES INTO @CODIGO, @COMPAÑIA,@CONTACTO, @PAIS
		

	END
	CLOSE CCLIENTES
	DEALLOCATE CCLIENTES
-----------------------
SELECT * FROM clientes

rollback
--Ejemplo: Trabajando con un cursor, listar los clientes registrados en la base de datos,
--incluya el nombre del país.
--(DESARROLLAR CON LA BASE DE DATOS NEPTUNO: PRODUCTOS, CATEGORIAS)
-- DECLARO VARIABLES DE TRABAJO

select  p.idproducto,p.nombreProducto, c.nombrecategoria
from productos p inner join categorias c
on p.idCategoria= c.idcategoria


DECLARE @ID VARCHAR(1), @NOMBRE VARCHAR(50), @CATEGORIA VARCHAR(50)
-- DECLARO EL CURSOR
DECLARE MI_CURSOR1 CURSOR 
FOR select  p.idproducto,p.nombreProducto, c.nombrecategoria
from productos p inner join categorias c
on p.idCategoria= c.idcategoria
-- ABRIR
OPEN MI_CURSOR1
-- LEER EL PRIMER REGISTRO
FETCH MI_CURSOR1 INTO @ID, @NOMBRE, @CATEGORIA
-- MIENTRAS PUEDA LEER EL REGISTRO
WHILE @@FETCH_STATUS=0
BEGIN
--IMPRIMIR EL REGISTRO
PRINT @ID + ','+@NOMBRE+','+@CATEGORIA
--LEER EL REGISTRO SIGUIENTE
FETCH MI_CURSOR1 INTO @ID, @NOMBRE, @CATEGORIA
END
-- CERRAR
CLOSE MI_CURSOR1
-- LIBERAR
DEALLOCATE MI_CURSOR1;
GO

----
PRINT CAST(@ID AS varchar(10)) + ','+@NOMBRE+','+@CATEGORIA  ---

---
---------------------------------------------------------------------------------------
USE DBRESERVA
SELECT * FROM PASAJERO
SELECT * FROM PAIS
--
DECLARE @idpasajero char(8),@pasajero char(20),
@pais char(10), @documento char(12)
declare micursor cursor
	for select pas.idpasajero,(pas.nombre + ' ' +pas.apaterno) as Pasajero,
	pai.nombre as Pais, pas.num_documento
	from pasajero pas inner join pais pai
		on pas.idpais=pai.idpais
open micursor
fetch micursor into @idpasajero,@pasajero ,@pais , @documento 
print 'CODIGO        PASAJERO               PAIS        DOCUMENTO '
PRINT '-----------------------------------------------------------'
-- AGREGAR LOS DETALLES
	WHILE @@FETCH_STATUS =0
		BEGIN
			PRINT @idpasajero+ space(5)+@pasajero+space(5)+
			@pais+space(2)+@documento
			fetch micursor into @idpasajero,@pasajero ,@pais , @documento 
			end
close micursor
deallocate micursor
-----------------------------------------------------------------------------------------------


/*
TEMARIO
1. Funciones definidas por el usuario
1.1. Funciones escalares
1.2. Funciones de tabla en línea
1.3. Funciones de tabla de multi sentencias
1.4. Limitaciones
2. Procedimientos almacenados
2.1. Especificar parámetros
2.2. Uso de cursores en procedimientos almacenados
2.3. Modificar datos con procedimientos almacenados
2.4. Transacciones en TRANSACT-SQL
2.4.1. Transacciones implícitas y explícitas
3. Triggers o disparadores
3.1. Definición de trigger
3.2. Creación de disparadores
3.2.1. Funcionamiento de los disparadores
3.3. Uso de INSTEAD OF

Funciones definidas por el usuario
Las funciones son rutinas que permiten encapsular sentencias TRANSACT-SQL que
se ejecutan frecuentemente.
Las funciones definidas por el usuario, en tiempo de ejecución de lenguaje
TRANSACT-SQL o común (CLR), acepta parámetros, realiza una acción, como un
cálculo complejo, y devuelve el resultado de esa acción como un valor. El valor de
retorno puede ser un escalar (único) valor o una tabla.
Funciones escalares
-------------------
Son aquellas funciones donde retornan un valor único: tipo de datos como int, Money,
varchar, real, etc. Pueden ser utilizadas en cualquier lugar, incluso, incorporada dentro
de las sentencias SQL.
--
CREATE FUNCTION [PROPIETARIO.] NOMBRE_FUNCION
([{@PARAMETER TIPO DE DATO [=DEFAULT]} [,..N]])
RETURNS VALOR_ESCALAR
[AS]
BEGIN
CUERPO DE LA FUNCION
RETURN EXPRESION_ESCALAR
END
Ejemplo: Crear una función que retorne el precio promedio de todos los productos
*/
USE neptuno
SELECT * FROM PRODUCTOS

CREATE FUNCTION DBO.PRECIOPROMEDIO() 
RETURNS DECIMAL
AS 
	BEGIN
		DECLARE @PROM DECIMAL
		SELECT @PROM=AVG(PRECIOUNIDAD)  FROM PRODUCTOS
		RETURN @PROM
	END  
GO
SELECT AVG(PRECIOUNIDAD) FROM PRODUCTOS 
-- MOSTRAR EL RESULTADO  
PRINT DBO.PRECIOPROMEDIO()
--
--Defina una función donde ingrese el id del empleado y 
--retorne la cantidad de  pedidos registrados en el  año 1994
---
SELECT * FROM PEDIDOS
SELECT * FROM detallesdepedidos

SELECT IDEMPLEADO, COUNT(*) AS CANTIDAD_PEDIDOS FROM PEDIDOS
GROUP BY IDEMPLEADO
ORDER BY 2 DESC

GO
---
CREATE FUNCTION PEDIDOSXEMP
(@ID INT)
RETURNS DECIMAL
AS 
	BEGIN
	DECLARE @Q DECIMAL=0  
	SELECT @Q=COUNT(*)  FROM PEDIDOS
	WHERE YEAR(FECHAPEDIDO)='1994' AND IDEMPLEADO=@ID
		IF @Q IS NULL
			SET @Q=0  
		RETURN @Q
	END
GO
-- VERIFICAR
SELECT  IDEMPLEADO, FECHAPEDIDO, IDPEDIDO
FROM PEDIDOS
	WHERE IDEMPLEADO= 5 AND YEAR(FECHAPEDIDO)= '1994'
--
PRINT DBO.PEDIDOSXEMP(5)

--   FUNCION DE TABLA EN LINEA

--Las funciones de tabla en línea son las funciones que devuelven la salida de una
--simple declaración SELECT. La salida se puede utilizar adentro de JOINS o querys
--como si fuera una tabla de estándar.
--La sintaxis para una función de tabla en línea es como sigue:
/*
CREATE FUNCTION [propietario.] nombre_funcion
([{ @parameter tipo de dato [ = default]} [,..n]])
RETURNS TABLE
[AS]
RETURN [(] Sentencia SQL [)]

*/
------Defina una función que liste los registros de los clientes,
--e incluya el nombre  del Pais.
use  DBRESERVA
create function pasajeroxpais1
(@pais as varchar(30))
returns table
as
return(select PAS.idpasajero,(PAS.nombre+ ' '+ PAS.apaterno) as Pasajero,PAI.nombre as Pais
from PASAJERO pas inner join  PAIS pai
		on pas.idpais=pai.idpais
		where pai.nombre=@pais)
go
-- probar
select * from dbo.pasajeroxpais1('URUGUAY')
select * from PASAJERO
select * from PAIS


-----
--Defina una función que liste los registros de los pedidos por un determinado  año,
--incluya el nombre del producto, el precio que fue vendido y la cantidad vendida
USE neptuno
CREATE FUNCTION DBO.PEDIDOSAÑO
(@Y INT)
RETURNS TABLE 
AS
		RETURN (SELECT P.IDPEDIDO AS 'PEDIDO',
			P.FECHAPEDIDO, PR.NOMBREPRODUCTO, DT.PRECIOUNIDAD AS 'PRECIO',
			DT.CANTIDAD
		FROM PEDIDOS P
			INNER JOIN DETALLESDEPEDIDOS DT
				ON P.IDPEDIDO=DT.IDPEDIDO 
			INNER JOIN PRODUCTOS PR 
				ON DT.IDPRODUCTO=PR.IDPRODUCTO 
					WHERE YEAR(FECHAPEDIDO) = @Y)
				GO


-- EJECUTANDO LA FUNCION
SELECT * FROM DBO.PEDIDOSAÑO(1996) 
GO
----------------------------------------------------------------------- continua lunes 22-02-2021-------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------
---- FUNCION DE TABLA MULTISENTENCIAS(lunes_22_02_2021)

--Son similares a los procedimientos almacenados excepto que vuelven una tabla. Este
--tipo de función se usa en situaciones donde se requiere más lógica y proceso. Lo que
--sigue es la sintaxis para unas funciones de tabla de multisentencias:
/*	CREATE FUNCTION [propietario.] nombre_funcion
([{@parameter tipo de dato [ = default]} [,..n]])
RETURNS TABLE
[AS]
BEGIN
Cuerpo de la función
RETURN
END
*/

---
--Defina una función que retorne el inventario de los productos
--registrados en  la base de datos.
alter FUNCTION DBO.INVENTARIO()
RETURNS @TABLA 
			TABLE  (IDPRODUCTO INT,
					NOMBRE VARCHAR(50),
					PRECIO DECIMAL,
					STOCK INT)
AS  
	BEGIN
		INSERT INTO @TABLA 
		SELECT IDPRODUCTO, 	NOMBREPRODUCTO,	PRECIOUNIDAD,  UNIDADESENEXISTENCIA
		FROM PRODUCTOS  
		RETURN
	END
GO

-- EJECUTANDO LA FUNCION 
SELECT * FROM DBO.INVENTARIO()
GO
--------------------------
-- Defina una función que permita generar un reporte de ventas por empleado,
--en un determinado año. En este proceso, la función debe retornar:
--los datos del  empleado, la cantidad de pedidos registrados y el monto total por empleado

select * from Pedidos
select * from detallesdepedidos

--------------------------
CREATE FUNCTION DBO.REPORTEVENTAS(@Y INT)
RETURNS @TABLA 
					TABLE(ID INT,
						NOMBRE VARCHAR(50), 
						CANTIDAD INT,
						MONTO DECIMAL)
AS  
BEGIN
	INSERT INTO @TABLA  SELECT E.IDEMPLEADO,E.APELLIDOS,
	COUNT(*),	SUM(PRECIOUNIDAD*CANTIDAD)
	FROM PEDIDOS PC inner JOIN DETALLESDEPEDIDOS PD 
		ON PC.IDPEDIDO = PD.IDPEDIDO
			inner 	JOIN EMPLEADOS E  
					ON E.IDEMPLEADO = PC.IDEMPLEADO
						WHERE YEAR(FECHAPEDIDO) = @Y
				GROUP BY E.IDEMPLEADO, E.APELLIDOS  
				RETURN
END 
GO

-- IMPRIMIR EL REPORTE DEL AÑO 1994 
SELECT * FROM DBO.REPORTEVENTAS(1996)
GO
/*  TENER EN CUENTA

Limitaciones
Las funciones definidas por el usuario tienen algunas restricciones. No todas las
sentencias SQL son válidas dentro de una función. Las listas siguientes enumeran las
operaciones válidas e inválidas de las funciones:
Válido:
• Las sentencias de asignación
• Las sentencias de Control de Flujo
• Sentencias SELECT y modificación de variables locales
• Operaciones de cursores sobre variables locales Sentencias INSERT,
UPDATE, DELETE con variables locales
Inválidas:
• Armar funciones no determinadas como GetDate()
• Sentencias de modificación o actualización de tablas o vistas
• Operaciones CURSOR FETCH que devuelven datos del cliente

*/
/*
Procedimientos Almacenados
-------------------------
Los procedimientos almacenados son grupos formados por instrucciones SQL y el
lenguaje de control de flujo. Cuando se ejecuta un procedimiento, se prepara un plan
de ejecución para que la subsiguiente ejecución sea muy rápida. Los procedimientos
almacenados pueden:
• Incluir parámetros
• Llamar a otros procedimientos
• Devolver un valor de estado a un procedimiento de llamada o lote para indicar el
éxito o el fracaso del mismo y la razón de dicho fallo.
• Devolver valores de parámetros a un procedimiento de llamada o lote
• Ejecutarse en SQL Server remotos
*/






-------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------
--Por ejemplo: Defina un procedimiento almacenado que liste todos los clientes
use neptuno
CREATE PROCEDURE USP_CLIENTES

AS
	SELECT IDCLIENTE AS CODIGO,
	NombreCompañia AS CLIENTE,
	DIRECCION,
	TELEFONO
		FROM CLIENTES
--------------------
EXEC USP_CLIENTES
----------------

--Por ejemplo: Cree un procedimiento almacenado que permita buscar los datos de los
--pedidos registrados en una determinada fecha. El procedimiento deberá definir un
--parámetro de entrada de tipo DateTime
select * from Pedidos
CREATE PROCEDURE USP_PEDIDOSFECHAS
@F1 DATETIME
AS
SELECT *
FROM Pedidos
WHERE FechaPedido = @F1
----
EXEC USP_PEDIDOSFECHAS @F1='10-01-1996'
EXEC USP_PEDIDOSFECHAS '10-01-1996'
---La sentencia ALTER PROCEDURE permite modificar el contenido del procedimiento
--almacenado. En este procedimiento, realizamos la consulta de pedidos entre un rango
--de dos fechas.
ALTER PROCEDURE USP_PEDIDOSFECHAS
@F1 DATETIME,
@F2 DATETIME
AS
SELECT *
FROM Pedidos
WHERE FechaPedido BETWEEN @F1 AND @F2
GO
--Para ejecutar
EXEC USP_PEDIDOSFECHAS @F1='10-01-1996', @F2='10-10-1996'
EXEC USP_PEDIDOSFECHAS '10-01-1996','10-10-1996'


--Para eliminar un procedimiento almacenado, ejecute la instrucción DROP PROCEDURE
DROP PROCEDURE USP_PEDIDOSFECHAS
-------------------------------
/*
Especificar parámetros
----------------------
Un procedimiento almacenado se comunica con el programa que lo llama mediante
sus parámetros. Cuando un programa ejecuta un procedimiento almacenado, es
posible pasarle valores mediante los parámetros del procedimiento.
Estos valores se pueden utilizar como variables estándar en el lenguaje de
programación TRANSACT-SQL. El procedimiento almacenado también puede
devolver valores al programa que lo llama mediante parámetros OUTPUT. Un
procedimiento almacenado puede tener hasta 2.100 parámetros, cada uno de ellos
con un nombre, un tipo de datos, una dirección y un valor predeterminado

Especificar el nombre del parámetro
----------------------------------
Cada parámetro de un procedimiento almacenado, debe definirse con un nombre
único. Los nombres de los procedimientos almacenados deben empezar por un solo
carácter @, como una variable estándar de TRANSACT-SQL, y deben seguir las
reglas definidas para los identificadores de objetos. El nombre del parámetro se puede
utilizar en el procedimiento almacenado para obtener y cambiar el valor del parámetro.
Especificar la dirección del parámetro
--------------------------------------
La dirección de un parámetro puede ser de entrada, que indica que un valor se pasa al
parámetro de entrada de un procedimiento almacenado o de salida, que indica que el
procedimiento almacenado devuelve un valor al programa que lo llama mediante un
parámetro de salida. El valor predeterminado es un parámetro de entrada.
Para especificar un parámetro de salida, debe indicar la palabra clave OUTPUT en la
definición del parámetro del procedimiento almacenado. El programa que realiza la
llamada también debe utilizar la palabra clave OUTPUT al ejecutar el procedimiento
almacenado, a fin de guardar el valor del parámetro en una variable que se pueda
utilizar en el programa que llama.

Especificar un valor de parámetro predeterminado
------------------------------------------------
Puede crear un procedimiento almacenado con parámetros opcionales especificando
un valor predeterminado para los mismos. Al ejecutar el procedimiento almacenado, se
utilizará el valor predeterminado si no se ha especificado ningún otro.
Es necesario especificar valores predeterminados, ya que el sistema devuelve un error
si en el procedimiento almacenado no se especifica un valor predeterminado para un
parámetro y el programa que realiza la llamada no proporciona ningún otro valor al
ejecutar el procedimiento.
Por ejemplo:
-----------
Cree un procedimiento almacenado que muestre los datos de los
pedidos, los productos que fueron registrados por cada pedido, el precio del producto y
la cantidad registrada por un determinado cliente y año. El procedimiento recibirá
como parámetro de entrada el código del cliente y el año. Considere que el parámetro
año tendrá un valor por defecto: el año del sistema.

*/


SELECT * FROM Pedidos
SELECT * FROM detallesdepedidos 
CREATE PROCEDURE USP_PEDIDOSCLIENTEAÑO
@ID VARCHAR(5),
@AÑO INT = 1994
AS
	SELECT PC.IDPEDIDO AS 'PEDIDO',
		FECHAPEDIDO, NOMBREPRODUCTO,PD.PRECIOUNIDAD AS '¨PRECIO',CANTIDAD
		FROM Pedidos PC inner JOIN detallesdepedidos PD
			ON PC.IdPedido = PD.idpedido 
			inner JOIN productos P
				ON PD.idproducto = P.idproducto
				WHERE YEAR(FechaPedido) = @AÑO
					AND IdCliente = @ID
----
--Como el procedimiento almacenado ha definido un valor por defecto al parámetro
--@año, podemos ejecutar el procedimiento enviando solamente el valor para el
--parámetro @id
EXEC USP_PEDIDOSCLIENTEAÑO @ID='WILMK'
EXEC USP_PEDIDOSCLIENTEAÑO @ID='WILMK', @AÑO=1995
---------
--Por ejemplo: Implemente un procedimiento almacenado que retorne la cantidad de
--pedidos y el monto total de pedidos, registrados por un determinado empleado
--(parámetro de entrada su id del empleado) y en determinado año (parámetro de
--entrada). Dicho procedimiento retornará la cantidad de pedidos y el monto total
------
-- corregir no va
create PROCEDURE USP_REPORTEPEDIDOSEMPLEADO
@ID INT,
@Y INT,
@Q INT OUTPUT,
@MONTO DECIMAL OUTPUT
AS
SELECT @Q= COUNT(*),
@MONTO = SUM(preciounidad*cantidad)
FROM Pedidos PC inner JOIN detallesdepedidos PD
ON PC.IdPedido = PD.idpedido
WHERE IdEmpleado =@ID AND YEAR(FechaPedido) = @Y

--Al ejecutar el procedimiento almacenado, primero declaramos las variables de retorno
--y al ejecutar, las variables de retorno se le indicara con la expresión OUTPUT
DECLARE @Q INT, @M DECIMAL
EXEC USP_REPORTEPEDIDOSEMPLEADO @ID=1,
@Y=1994,
@Q=@Q OUTPUT,
@MONTO=@M OUTPUT
PRINT 'CANTIDAD DE PEDIDOS COLOCADOS:' + STR(@Q)
PRINT 'MONTO PERCIBIDO:'+STR(@M)
GO
select * from Pedidos
where   IdEmpleado=5 and YEAR(fechapedido)='1994'
/* 
   Uso de cursores en procedimientos almacenados
   ---------------------------------------------
Los cursores son especialmente útiles en procedimientos almacenados. Permiten
llevar a cabo la misma tarea utilizando sólo una consulta que, de otro modo, requeriría
varias. Sin embargo, todas las operaciones del cursor deben ejecutarse dentro de un
solo procedimiento. Un procedimiento almacenado no puede abrir, recobrar o cerrar un
cursor que no esté declarado en el procedimiento. El cursor no está definido fuera del
alcance del procedimiento almacenado.
Por ejemplo: Implemente un procedimiento almacenado que imprimir cada uno de los
registros de los productos, donde al finalizar, visualice el total del inventario (Suma de
cantidad de productos)
*/
	create  PROCEDURE USP_INVENTARIO
	AS
	-- DECLARACION DE VARIABLES PARA EL CURSOR
	DECLARE @ID INT, @NOMBRE VARCHAR(255), @PRECIO DECIMAL, @ST INT,
	@INV INT
	SET @INV=0
	-- DECLARACIÓN DEL CURSOR
	DECLARE CPRODUCTO CURSOR FOR
							SELECT IDPRODUCTO,
									NOMBREPRODUCTO,
									PRECIOUNIDAD,
									UNIDADESENEXISTENCIA
							FROM PRODUCTOS
	-- APERTURA DEL CURSOR
	OPEN CPRODUCTO
	-- LECTURA DE LA PRIMERA FILA DEL CURSOR
	FETCH CPRODUCTO INTO @ID, @NOMBRE, @PRECIO, @ST
		WHILE (@@FETCH_STATUS = 0 )
			BEGIN
	-- IMPRIMIR                     
			PRINT STR(@ID) + SPACE(5) + @NOMBRE + SPACE(5) +
			STR(@PRECIO) + SPACE(5) + STR(@ST)
			-- ACUMULAR
			SET @INV += @ST
	-- LECTURA DE LA SIGUIENTE FILA DEL CURSOR
		FETCH CPRODUCTO INTO @ID, @NOMBRE, @PRECIO, @ST
		END
	-- CIERRE DEL CURSOR
	CLOSE CPRODUCTO
	-- LIBERAR LOS RECURSOS
	DEALLOCATE CPRODUCTO
	PRINT 'INVENTARIO DE PRODUCTOS:' + STR(@INV)
	GO
	-- comprobar
	SELECT SUM(UNIDADESENEXISTENCIA)
	FROM productos

	exec USP_INVENTARIO

	select SUM(unidadesenexistencia)
	from productos
	----
	----
	--En el siguiente ejemplo, definimos un procedimiemto que permita generar un reporte
--de los pedidos realizados por un empleado en cada año, totalizando el monto de sus
--operaciones por cada año.
CREATE PROCEDURE USP_REPORTEPEDIDOSXAÑOXEMPLEADO
@EMP INT=5
AS
-- DECLARACIÓN DE VARIABLES DE TRABAJO
DECLARE @Y INT, @Y1 INT, @PEDIDO INT, @MONTO DECIMAL, @TOTAL DECIMAL
SET @TOTAL=0
-- DECLARACIÓN DEL CURSOR
DECLARE MI_CURSOR CURSOR FOR
		SELECT YEAR(FECHAPEDIDO) AS 'AÑO',
					PC.IDPEDIDO,
					SUM(PRECIOUNIDAD*CANTIDAD) AS MONTO
					FROM PEDIDOS PC
						JOIN detallesdepedidos PD
							ON PC.IDPEDIDO=PD.IDPEDIDO
						WHERE IDEMPLEADO = @EMP
				GROUP BY YEAR(FECHAPEDIDO), PC.IDPEDIDO
					ORDER BY 1
-- APERTURA DEL CURSOR
	OPEN MI_CURSOR
-- LECTURA DEL PRIMER REGISTRO
	FETCH MI_CURSOR INTO @Y, @PEDIDO, @MONTO
-- ASIGNACIÓN DEL VALOR INICIAL DE @Y EN LA VARIABLE @Y1
	SET @Y1 = @Y
-- IMPRIMIR EL PRIMER AÑO
	PRINT 'AÑO:' + CAST(@Y1 AS VARCHAR)
-- RECORRER EL CURSOS MIENTRAS HAYAN REGISTROS
	WHILE @@FETCH_STATUS=0
		BEGIN
			IF(@Y = @Y1)
			BEGIN
		-- ACUMULAR
			SET @TOTAL += @MONTO
			END
			ELSE
			BEGIN
			PRINT 'AÑO:' + CAST(@Y1 AS VARCHAR) + SPACE(2)+
			'IMPORTE: ' + CAST(@TOTAL AS VARCHAR)
			PRINT 'AÑO:' + CAST(@Y AS VARCHAR)
			SET @Y1=@Y
			SET @TOTAL=@MONTO
		END
-- IMPRIMIR EL REGISTRO
		PRINT CAST(@PEDIDO AS VARCHAR) + SPACE(5)+ CAST(@MONTO AS VARCHAR)
-- LECTURA DEL SIGUIENTE REGISTRO
	FETCH MI_CURSOR INTO @Y, @PEDIDO, @MONTO
	END
-- CERRAR EL CURSOR
	CLOSE MI_CURSOR
	-- LIBERAR EL RECURSO
	DEALLOCATE MI_CURSOR;
	PRINT ' AÑO:' + CAST(@Y1 AS VARCHAR) + SPACE(2)+ 'IMPORTE: ' + STR(@TOTAL)
go

SELECT * FROM Pedidos
SELECT * FROM DETALLESDEPEDIDOS
--Al ejecutar el procedimiento almacenado, se le envía el parámetro que representa el id
--del empleado, imprimiendo su record de ventas de pedidos por año
	exec USP_REPORTEPEDIDOSXAÑOXEMPLEADO 1

---Por ejemplo( bd_noche_22_02_2021)
   ------------
--defina un procedimiento almacenado para insertar un registro de la tabla
--Clientes, en este procedimiento, definiremos parámetros de entrada que representan
--los campos de la tabla.

CREATE PROCEDURE USP_INSERTACLIENTE
@ID VARCHAR(5),
@NOMBRE VARCHAR(50),
@DIRECCION VARCHAR(100),
@PAIS varCHAR(100),
@FONO VARCHAR(30)
AS
INSERT INTO CLIENTES(idCliente,NombreCompañia, Direccion,Pais,Telefono)
VALUES(@ID, @NOMBRE, @DIRECCION, @PAIS, @FONO)
-----
EXEC USP_INSERTACLIENTE 'ABCDE', 'JUAN CARLOS MEDINA',
'CALLE 25 NO 123','FRANCIA','5450555'

SELECT * FROM clientes
---------------------------------------------------------------------------------------------------------------------------------------
--TRANSACCIONES EN TRANSACT-SQL
--------------------------------
/*
Una transacción es un conjunto de operaciones TRANSACT SQL que se ejecutan
como un único bloque, es decir, si falla una operación TRANSACT SQL fallan todas.
Si una transacción tiene éxito, todas las modificaciones de los datos realizadas
durante la transacción se confirman y se convierten en una parte permanente de la
base de datos. Si una transacción encuentra errores y debe cancelarse o revertirse, se
borran todas las modificaciones de los datos.
El ejemplo clásico de transacción es un registro de pedidos, donde agregamos un
registro a la tabla pedidoscabe y agregamos un registro a la tabla pedidosdeta
(producto solicitado por el cliente) y descontamos el stock del producto solicitado en el
pedido.
*/
--CARGAR BASE DE VENTAS_ Y PASAR EL PROGRAMA
CREATE PROCEDURE USP_AGREGAPEDIDO
-- PARÁMETROS DE PEDIDOSCABE
@IDPED INT,
@IDCLI VARCHAR(5),
@IDEMP INT,
@FECPED DATETIME,
-- PARÁMETROS DE PEDIDOSDETA
@IDPROD INT,
@PRE DECIMAL,
@CANT INT
AS
-- AGREGANDO UN REGISTRO A PEDIDOSCABE
INSERT INTO PEDIDOS(IDPEDIDO,IDCLIENTE,
IDEMPLEADO,FECHAPEDIDO)
VALUES(@IDPED, @IDCLI, @IDEMP, @FECPED)
-- AGREGANDO UN REGISTRO A PEDIDOSDETA
INSERT INTO DETALLESDEPEDIDOS(IDPEDIDO, IDPRODUCTO,
PRECIOUNIDAD,CANTIDAD, DESCUENTO)
VALUES(@IDPED, @IDPROD, @PRE, @CANT, 0)
-- DESCONTANDO EL STOCK DE PRODUCTOS
UPDATE PRODUCTOS SET UNIDADESENEXISTENCIA -=@CANT
WHERE IDPRODUCTO = @IDPROD
--
---Esta forma de procesar las operaciones de pedidos sería errónea, ya que cada
--instrucción se ejecutaría y confirmaría de forma independiente, por lo que un error en
--alguna de las operaciones dejaría los datos erróneos en la base de datos.
/*
Transacciones implícitas y explicitas
-------------------------------------
Para agrupar varias sentencias TRANSACT SQL en una única transacción,
disponemos de los siguientes métodos:
• Transacciones explícitas: Cada transacción se inicia explícitamente con la
instrucción BEGIN TRANSACTION y se termina explícitamente con una
instrucción COMMIT o ROLLBACK.
• Transacciones implícitas: Se inicia automáticamente una nueva transacción
cuando se ejecuta una instrucción que realiza modificaciones en los datos, pero
cada transacción se completa explícitamente con una instrucción COMMIT o
ROLLBACK.

Sintaxis para el control de las transacciones
---------------------------------------------
-- Inicio de transacción con nombre
BEGIN TRAN NombreTransaccion
/*Bloque de instrucciones a ejecutar en la Transacción*/
COMMIT TRAN NombreTransaccion--Confirmación de la transacción.
ROLLBACK TRAN NombreTransaccion--Reversión de la transacción.
*/
--------------------------------------------------------------
BEGIN TRY
 /*Bloque de instrucciones a validar*/
END TRY 
--------------------------------------------------------------
BEGIN CATCH
 /*Bloque de instrucciones que se ejecutan si ocurre un ERROR*/
END CATCH
--------------------------------------------------------------
--Para un mejor control de los errores, ejecutamos los procesos dentro del bloque TRY
--CATCH, donde en caso que las operaciones hayan tenido éxito, ejecutará COMMIT
--TRAN, pero en caso de error CATCH, ejecutará ROLLBACK TRAN, tal como se
--muestra en el siguiente ejercicio.

------------------------------------------


----





---------------------------------------------------------------
SELECT * FROM ARTICULOS
SELECT * FROM VENTAS

use SIS_VENTAS
CREATE PROCEDURE  SP_OBTENER_CLIENTES 
AS 
BEGIN
	SELECT * FROM CLIENTES
END
EXEC SP_OBTENER_CLIENTES
DROP  proc SP_OBTENER_CLIENTES
SELECT * FROM ARTICULOS

---------------------------------------------------------------------
select * from articulos
CREATE PROCEDURE  SP_AGREGAR_STOCK
(@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN
 UPDATE ARTICULOS SET STOCK = STOCK + @CANTIDAD
	WHERE IDARTICULO= @IDARTICULO
END
SELECT * FROM ARTICULOS WHERE IDARTICULO = 1
 SP_AGREGAR_STOCK 1, 30  -- SE MODIFICO A 16 DE 10
 SP_AGREGAR_STOCK 1, -100
 -- HAY Q SOLUCIONAR EL NEGATIVO MODIFI EL PROC ANTER
 ALTER PROCEDURE  SP_AGREGAR_STOCK
(@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN
		IF @CANTIDAD >0 BEGIN
							UPDATE ARTICULOS SET STOCK = STOCK + @CANTIDAD
							WHERE IDARTICULO= @IDARTICULO
						END
        ELSE
			BEGIN
				RAISERROR('LA CANTIDAD NO ES VALIDA', 16,1)
			END
END
-- PROBAMOS
SELECT * FROM ARTICULOS WHERE IDARTICULO = 1
 SP_AGREGAR_STOCK 1, 10
 SP_AGREGAR_STOCK 1, -10

 -- INCORPORAR NUESTROS PROD ALM A NUESTRAS APLICACIONES

 CREATE PROCEDURE  SP_AGREGAR_VENTA
(
@DNI INT,
@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN
	INSERT INTO VENTAS(DNI,IDARTICULO,CANTIDAD,FECHA,IMPORTE)
	VALUES(@DNI,@IDARTICULO,@CANTIDAD, GETDATE(),0)

 END
 --
 -- PROBAR
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SELECT * FROM ARTICULOS

 TRUNCATE TABLE VENTAS

  SP_AGREGAR_VENTA  1,1,4
 --

 ALTER  PROCEDURE  SP_AGREGAR_VENTA
(@DNI INT,
@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN
	DECLARE @PU DECIMAL(6,2)
	SELECT @PU= PRECIO FROM ARTICULOS 
		WHERE IDARTICULO=@IDARTICULO
	

	INSERT INTO VENTAS(DNI,IDARTICULO,CANTIDAD,FECHA,IMPORTE)
	VALUES(@DNI,@IDARTICULO,@CANTIDAD, GETDATE(),@PU* @CANTIDAD)

 END

 -- PROBAR
 SELECT * FROM ARTICULOS
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SP_AGREGAR_VENTA  1,1,2
 -- AGREGAR  BEGIN TRY / CATCH
 ALTER PROCEDURE  SP_AGREGAR_VENTA
(@DNI INT,
@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN	
		BEGIN TRY
			DECLARE @PU DECIMAL(6,2)
			SELECT @PU= PRECIO FROM ARTICULOS 
			WHERE IDARTICULO=@IDARTICULO
		
			INSERT INTO VENTAS(DNI,IDARTICULO,CANTIDAD,FECHA,IMPORTE)
			VALUES(@DNI,@IDARTICULO,@CANTIDAD, GETDATE(),@PU* @CANTIDAD)

			UPDATE ARTICULOS SET STOCK= STOCK- @CANTIDAD	
							WHERE IDARTICULO=@IDARTICULO

		END TRY
		BEGIN CATCH
			RAISERROR('NO SE PUDO AGREGAR LA VENTA', 16,1)
		END CATCH
			
 END
 -- PROBAR
 SELECT * FROM ARTICULOS
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SP_AGREGAR_VENTA  1,1,10
 -- TRATAMOS VENDER MAS DE LO QUE CONTIENE EL STOCK
	SELECT * FROM ARTICULOS
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SP_AGREGAR_VENTA  1,1,100

 ---
 

 -- 27-05-18 TRANSACIOMES  PROBAR 
 ALTER PROCEDURE  SP_AGREGAR_VENTA
(@DNI INT,
@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN	
		BEGIN TRY
				BEGIN TRANSACTION
			DECLARE @PU DECIMAL(6,2)
			SELECT @PU= PRECIO FROM ARTICULOS 
			WHERE IDARTICULO=@IDARTICULO

			INSERT INTO VENTAS(DNI,IDARTICULO,CANTIDAD,FECHA,IMPORTE)
			VALUES(@DNI,@IDARTICULO,@CANTIDAD, GETDATE(),@PU* @CANTIDAD)

			UPDATE ARTICULOS SET STOCK= STOCK- @CANTIDAD	
			WHERE IDARTICULO=@IDARTICULO
			COMMIT TRANSACTION

		END TRY
		BEGIN CATCH
			ROLLBACK TRANSACTION
			RAISERROR('NO SE PUDO AGREGAR LA VENTA', 16,1)
		END CATCH
			
 END
 -- PROBAR ANTES APLICAR SELECT
 SELECT * FROM ARTICULOS
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SP_AGREGAR_VENTA  1,1,100
 
 -- EJEMPLO 2 LLAMAR PROC DENTRO DE OTRO PROC
 ALTER PROCEDURE  SP_AGREGAR_VENTA
(@DNI INT,
@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN	
		BEGIN TRY
				BEGIN TRANSACTION
			DECLARE @PU DECIMAL(6,2)
			SELECT @PU= PRECIO FROM ARTICULOS 
			WHERE IDARTICULO=@IDARTICULO

			INSERT INTO VENTAS(DNI,IDARTICULO,CANTIDAD,FECHA,IMPORTE)
			VALUES(@DNI,@IDARTICULO,@CANTIDAD, GETDATE(),@PU* @CANTIDAD)

			EXEC  SP_DESCONTAR_STOCK @IDARTICULO ,@CANTIDAD 
			COMMIT TRANSACTION

		END TRY
		BEGIN CATCH
		    IF @@TRANCOUNT > 0 BEGIN
								ROLLBACK TRANSACTION
								END
			RAISERROR('NO SE PUDO AGREGAR LA VENTA', 16,1)
		END CATCH
			
 END
 -- EJECUTAR
 SELECT * FROM ARTICULOS
 SELECT * FROM VENTAS ORDER BY IDVENTA DESC
 SP_AGREGAR_VENTA  1,1,12

 
 -- CREAR EL PROC
 create PROCEDURE  SP_DESCONTAR_STOCK
(@IDARTICULO INT,
@CANTIDAD INT
)
AS 
BEGIN
		BEGIN TRY
				BEGIN TRANSACTION
			
			 UPDATE ARTICULOS SET STOCK= STOCK- @CANTIDAD	
			 WHERE IDARTICULO=@IDARTICULO
			
			COMMIT TRANSACTION

		END TRY
		BEGIN CATCH
			ROLLBACK TRANSACTION
			RAISERROR('NO SE PUDO DESCONTAR EL STOCK', 16,1)
		END CATCH
END

------------------------------------------------
/*
TRIGGERS O DISPARADORES
Los disparadores pueden usarse para imponer la integridad de referencia de los datos
en toda la base de datos. Los disparadores también permiten realizar cambios “en
cascada” en tablas relacionadas, imponer restricciones de columna más complejas
que las permitidas por las reglas, compara los resultados de las modificaciones de
datos y llevar a cabo una acción resultante.
3.1.5.1 Definición del disparador
----------------------------------
Un disparador es un tipo especial de procedimiento almacenado que se ejecuta
cuando se insertan, eliminan o actualizan datos de una tabla especificada. Los
disparadores pueden ayuda a mantener la integridad de referencia de los datos
conservando la consistencia entre los datos relacionados lógicamente de distintas
tablas. Integridad de referencia significa que los valores de las llaves primarias y los
valores correspondientes de las llaves foráneas deben coincidir de forma exacta.
La principal ventaja de los disparadores es que son automáticos: funcionan cualquiera
sea el origen de la modificación de los datos. Cada disparador es específico de una o
más operaciones de modificación de datos, UPDATE, INSERT o DELETE. El
disparador se ejecuta una vez por cada instrucción.


Creación de disparadores
-----------------------
Un disparador es un objeto de la base de datos. Cuando se crea un disparador, se
especifica la tabla y los comandos de modificación de datos que deben “disparar” o
activar el disparador. Luego, se indica la acción o acciones que debe llevar a cabo un
disparador.  
		
		CREATE TRIGGER [ esquema. ]nombre_trigger
			ON { Tabla | Vista }
			{ FOR | AFTER | INSTEAD OF }
			{ [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] }
			[ WITH APPEND ]
			[ NOT FOR REPLICATION ]
			AS
			sentencia sql [ ; ] 

A continuación, se muestra un ejemplo sencillo. Este disparador imprime un mensaje
cada vez que alguien trata de insertar, eliminar o actualizar datos de la tabla Productos 

*/

CREATE TRIGGER TX_Productos
ON .Productos
FOR INSERT, UPDATE, DELETE
AS
PRINT 'Actualizacion de los registros de Productos'
SELECT * FROM productos
--- PROBAR
INSERT productos VALUES(78,'Inka Kola',12,1,'24 botellas',3.5,50,0,0,0,'')
--
--Para modificar el TRIGGER, se utiliza la siguiente sintaxis:
ALTER TRIGGER TX_PRODUCTOS
ON PRODUCTOS
FOR INSERT, UPDATE, DELETE
AS
PRINT 'ACTUALIZACION DE LOS REGISTROS DE PRODUCTOS'
--Para borrar un TRIGGER, se utiliza la siguiente sintaxis:
DROP TRIGGER TX_PRODUCTOS

/*
Funcionamiento de los Disparadores
A. Disparador de Inserción
--------------------------
Cuando se inserta una nueva fila en una tabla, SQL Server inserta los nuevos valores
en la tabla INSERTED el cual es una tabla del sistema. Está tabla toma la misma
estructura del cual se originó el TRIGGER, de tal manera que se pueda verificar los
datos y ante un error podría revertirse los cambios.
Cree un TRIGGER que permita insertar los datos de un Producto siempre y cuando la
descripción o nombre del producto sea único


*/
CREATE TRIGGER TX_PRODUCTO_INSERTA
ON PRODUCTOS
FOR INSERT
AS
IF (SELECT COUNT (*) FROM INSERTED, PRODUCTOS
WHERE INSERTED.nombreProducto = PRODUCTOS.nombreProducto) >1
BEGIN
ROLLBACK TRANSACTION
PRINT 'LA DESCRIPCION DEL PRODUCTO SE ENCUENTRA REGISTRADO'
END
ELSE
PRINT 'EL PRODUCTO FUE INGRESADO EN LA BASE DE DATOS' 

--
INSERT productos VALUES(79,'Coca Cola',12,1,'24 botellas',3.5,50,0,0,0,'')

INSERT productos VALUES(78,'Coca Cola',12,1,'24 botellas',3.5,50,0,0,0,'')
SELECT * FROM productos

--En este ejemplo, verificamos el número de productos que tienen la misma descripción
--y de encontrarse más de un registro de productos no se deberá permitir ingresar los
--datos del producto. Este disparador imprime un mensaje si la inserción se revierte y
--otro si se acepta. 
/*
Disparador de Eliminación
------------------------
Cuando se elimina una fila de una tabla, SQL Server inserta los valores que fueron
eliminados en la tabla DELETED el cual es una tabla del sistema. Está tabla toma la
misma estructura del cual se origino el TRIGGER, de tal manera que se pueda
verificar los datos y ante un error podría revertirse los cambios. En este caso, la
reversión de los cambios significará restaurar los datos eliminados.
Cree un TRIGGER el cual permita eliminar Clientes los cuales no han registrado algún
pedido. De eliminarse algún Cliente que no cumpla con dicha condición la operación
no deberá ejecutarse. 

*/

CREATE TRIGGER TX_ELIMINA_ELIMINA
ON CLIENTES
FOR DELETE
AS
IF EXISTS (SELECT * FROM PEDIDOS
WHERE IDCLIENTE = (SELECT IDCLIENTE FROM DELETED) )
BEGIN
ROLLBACK TRANSACTION
PRINT 'EL CLIENTE TIENE REGISTRADO POR LO MENOS 1 PEDIDOS'
END 
--

SELECT * FROM Pedidos ORDER BY 2
SELECT * FROM clientes ORDER BY 1

INSERT INTO CLIENTES VALUES('ABCD','COMPAÑIA DER PRUEBA','PEDRO DIAZ','Propietario',' ','','','','','','')

delete clientes where idCliente='ABCD'
delete clientes where idCliente='BOTTM'

--En este ejemplo, verificamos si el cliente tiene pedidos registrados, de ser así la
--operación deberá ser cancelada. 
-- Disparador de Actualización
 --------------------------
--Cuando se actualiza una fila de una tabla, SQL Server inserta los valores que antiguos
--en la tabla DELETED y los nuevos valores los inserta en la tabla INSERTED. Usando 
--estas dos tablas se podrá verificar los datos y ante un error podrían revertirse los
--cambios.
--Cree un TRIGGER que valide el precio unitario y su Stock de un producto, donde
--dichos datos sean mayores a cero. 


CREATE TRIGGER TX_PRODUCTO_ACTUALIZA
ON PRODUCTOS
FOR UPDATE
AS
IF (SELECT PRECIOUNIDAD FROM INSERTED) <=0 OR
(SELECT UNIDADESENEXISTENCIA FROM INSERTED)<=0
BEGIN
PRINT 'EL PRECIO O UNIDADESENEXISTENCIA DEBEN SER MAYOR A CERO'
ROLLBACK TRANSACTION
END 
-----
SELECT * FROM productos

UPDATE productos SET precioUnidad=0
	WHERE idproducto=1

--Cree un TRIGGER que bloquee actualizar el id del producto en el proceso de
--actualización.
CREATE TRIGGER TX_PRODUCTO_ACTUALIZA_ID
ON PRODUCTOS
FOR UPDATE
AS
IF UPDATE(IDPRODUCTO)
BEGIN
PRINT 'NO SE PUEDE ACTUALIZAR EL ID DEL PRODUCTO'
ROLLBACK TRANSACTION
END 

--
select * from ventas
delete   ventas where idventa=3 
--
 use sis_ventas
 ---
 CREATE TRIGGER TX_PRODUCTO_ACTUALIZA_ID
ON ventas
FOR UPDATE
AS
IF UPDATE(idventa)
BEGIN
PRINT 'NO SE PUEDE ACTUALIZAR EL ID DE LA VENTA'
ROLLBACK TRANSACTION
END
--
select * from ventas
UPDATE   ventas SET IDVENTA=200 where idventa=5

---


----------
-- triggers
create table historial_cliente
(
idcliente char(5) not null,
nombrecompañia varchar(40) null,
direccion varchar(60) null,
ciudad varchar(15) null,
pais varchar(15) null,
transa char(1) null,
fectra datetime,
usuario varchar(50) null,
estacion varchar(50) null
)
--trigger inserta
create trigger inserta_hc
on clientes
for insert
as
insert historial_cliente(idcliente,nombrecompañia,direccion,
ciudad,pais,transa,fectra,usuario,estacion)
select idcliente,nombrecompañia,direccion,ciudad,pais,'I',
		getdate(),system_user,host_name()
from inserted
go
-- prueba
insert clientes values('Otrix','Otrix sac','Rita miller',
'Representante de ventas','lejos 123','Lima','','70563',
'Peru','967725383','')
--
-- trigger actualiza
create trigger actualiza_hc
on clientes
for update
as
insert historial_cliente(idcliente,nombrecompañia,direccion,
ciudad,pais,transa,fectra,usuario,estacion)
select idcliente,nombrecompañia,direccion,ciudad,pais,'U',
		getdate(),system_user,host_name()
from inserted
go
--
-- prueba actualizacion
update clientes set NombreCompañia='Zoltrix sac'
	where idcliente='Otrix'
--
select * from clientes
select * from historial_cliente
-- ELIMINA TRIGGER
create trigger elimina_hc
on clientes
for delete
as
insert historial_cliente(idcliente,nombrecompañia,direccion,
ciudad,pais,transa,fectra,usuario,estacion)
select idcliente,nombrecompañia,direccion,ciudad,pais,'D',
		getdate(),system_user,host_name()
from deleted
go
--
--prueba elimina trigger
delete clientes where idcliente='Otrix'
--
SELECT * FROM clientes

--CREA UN TRIGGER QUE PERMITA DESCONTAR EL STOCK EN
-- LA TABLA PRODUCTOS,SEGUN LA CANTIDAD DE PRODUCTOS
-- PEDIDOS
CREATE TRIGGER DESCUENTA_STOCK
ON DETALLESDEPEDIDOS
FOR INSERT
AS
--@CANTIDAD ALMACENA EL NUMERO DE PRODUCTOS A PEDIR
--@UNIDADES ALMACENA EL STOCK ACTUAL EN LA TABLA PRODUCTOS
DECLARE @CANTIDAD INTEGER
DECLARE @UNIDADES INTEGER
--ASIGNAMOS A LA VARIABLE EL CONTENIDO DEL CAMPO
SELECT @CANTIDAD= D.CANTIDAD
FROM detallesdepedidos D INNER JOIN inserted I
ON D.idpedido=I.idpedido
--ASIGNAMOS A LA VARIABLE EL CONTENIDO DEL CAMPO
SELECT @UNIDADES=P.UNIDADESENEXISTENCIA
FROM productos P INNER JOIN INSERTED I
ON P.idproducto=I.idproducto
BEGIN TRANSACTION
-- VERIFICAMOS LA CANTIDAD PEDIDA
IF(@CANTIDAD<= @UNIDADES)
	BEGIN
		-- ACTUALIZA EL STOCK
		UPDATE productos
		SET unidadesEnExistencia=@UNIDADES - @CANTIDAD
			FROM productos P INNER JOIN inserted I
				ON P.idproducto= I.idproducto
				COMMIT TRANSACTION
				PRINT 'STOCK ACTUALIZADO'
				END
		ELSE
			BEGIN
			-- DESHACEMOS LA TRANSACCION
			PRINT 'NO EXISTEN SUFICIENTES PRODUCTOS PARA EL PEDIDO'
			ROLLBACK
			END
	GO
--
-- PROBAMOS EL TRIGGER CON LOS SGTES. SCRIPT
INSERT productos VALUES(78,'Inka Kola',12,1,'24 botellas',3.5,50,0,0,0,'')

-- creamos un pedido con su respectivo detalle
insert Pedidos values(9999,'WOLZA',1,'05/06/2001','05/08/2001',
'05/07/2001',2,83.5,'Recibe ok','av.cto grande 127',
'Lima','','','Peru')
-- pedido detalle
insert detallesdepedidos values(9999,78,3.5,30,0)
--
------------------------- Disparador (Instead Off y after)-------------------
Hasta el momento hemos aprendido que un trigger se crea sobre una tabla específica para un
evento (inserción, eliminación o actualización).

También podemos especificar el momento de disparo del trigger. El momento de disparo indica 
que las acciones (sentencias) del trigger se ejecuten luego de la acción (insert, delete o update)
que dispara el trigger o en lugar de la acción.

La sintaxis para ello es:

 create trigger NOMBREDISPARADOR
  on NOMBRETABLA o VISTA
  MOMENTODEDISPARO-- after o instead of
  ACCION-- insert, update o delete
 as 
  SENTENCIAS
Entonces, el momento de disparo especifica cuando deben ejecutarse las acciones (sentencias)
lo dispara.

Si no especificamos el momento de disparo en la creación del trigger, por defecto se establece 
como "after", es decir, las acciones que el disparador realiza se ejecutan luego del suceso disparador. Hasta el momento, todos los disparadores que creamos han sido "after".

Los disparadores "instead of" se ejecutan en lugar de la acción desencadenante, es decir, 
cancelan la acción desencadenante (suceso que disparó el trigger) reemplazándola por otras acciones.
--
Veamos un ejemplo. Una empresa almacena los datos de sus empleados en una tabla "empleados" y 
en otra tabla "clientes" los datos de sus clientes. Se crea una vista que muestra los datos de
ambas tablas:

 create view vista_empleados_clientes
 as
  select documento,nombre, domicilio, 'empleado' as condicion from empleados
  union
   select documento,nombre, domicilio,'cliente' from clientes;
Creamos un disparador sobre la vista "vista_empleados_clientes" para inserción, que redirija 
las inserciones a la tabla correspondiente:

 create trigger DIS_empleadosclientes_insertar
  on vista_empleados_clientes
  instead of insert
  as
    insert into empleados 
     select documento,nombre,domicilio
     from inserted where condicion='empleado'

    insert into clientes
     select documento,nombre,domicilio
     from inserted where condicion='cliente';
El disparador anterior especifica que cada vez que se ingresen registros en la 
vista "vista_empleados_clientes", en vez de (instead of) realizar la acción (insertar en la vista),
se ejecuten las sentencias del trigger, es decir, se ingresen los registros en las tablas
correspondientes.

Entonces, las opciones de disparo pueden ser:

a) "after": el trigger se dispara cuando las acciones especificadas (insert, delete y/o update) 
son ejecutadas; todas las acciones en cascada de una restricción "foreign key" y las comprobaciones 
de restricciones "check" deben realizarse con éxito antes de ejecutarse el trigger. Es la opción por
defecto si solamente colocamos "for" (equivalente a "after").

La sintaxis es:

 create trigger NOMBREDISPARADOR
  on NOMBRETABLA
  after | for-- son equivalentes
  ACCION-- insert, update o delete
 as 
  SENTENCIAS
b) "instead of": sobreescribe la acción desencadenadora del trigger. Se puede definir solamente 
un disparador de este tipo para cada acción (insert, delete o update) sobre una tabla o vista.

Sintaxis:

 create trigger NOMBREDISPARADOR
  on NOMBRETABLA o VISTA
  instead of
  ACCION-- insert, update o delete
 as 
  SENTENCIAS
Consideraciones:

- Se pueden crear disparadores "instead of" en vistas y tablas.

- No se puede crear un disparador "instead of" en vistas definidas "with check option"
--
No se puede crear un disparador "instead of delete" y "instead of update" sobre tablas que 
tengan una "foreign key" que especifique una acción "on delete cascade" y "on update cascade"
respectivamente.

- Los disparadores "after" no pueden definirse sobre vistas.

- No pueden crearse disparadores "after" en vistas ni en tablas temporales; pero pueden 
referenciar vistas y tablas temporales.

- Si existen restricciones en la tabla del disparador, se comprueban DESPUES de la
ejecución del disparador "instead of" y ANTES del disparador "after". Si se infringen
las restricciones, se revierten las acciones del disparador "instead of"; en el caso del
disparador "after", no se ejecuta.

Servidor de SQL Server instalado en forma local.
Ingresemos el siguiente lote de comandos en el SQL Server Management Studio:

if object_id('empleados') is not null
  drop table empleados;
if object_id('clientes') is not null
  drop table clientes;

create table empleados(
  documento char(8) not null,
  nombre varchar(30),
  domicilio varchar(30),
  constraint PK_empleados primary key(documento)
);

create table clientes(
  documento char(8) not null,
  nombre varchar(30),
  domicilio varchar(30),
  constraint PK_clientes primary key(documento)
);

-- Eliminamos la siguiente vista:
if object_id('vista_empleados_clientes') is not null
  drop view vista_empleados_clientes;

go

-- Creamos una vista que muestra los datos de ambas tablas:
create view vista_empleados_clientes
 as
  select documento,nombre, domicilio, 'empleado' as condicion from empleados
  union
   select documento,nombre, domicilio,'cliente' from clientes;

go

-- Creamos un disparador sobre la vista "vista_empleados_clientes" para inserción,
-- que redirija las inserciones a la tabla correspondiente:
create trigger DIS_empl_clie_insertar
  on vista_empleados_clientes
  instead of insert
  as
    insert into empleados 
     select documento,nombre,domicilio
     from inserted where condicion='empleado'

    insert into clientes
     select documento,nombre,domicilio
     from inserted where condicion='cliente';

go

-- Ingresamos un empleado y un cliente en la vista:
insert into vista_empleados_clientes values('22222222','Ana Acosta', 'Avellaneda 345','empleado');
insert into vista_empleados_clientes values('23333333','Bernardo Bustos', 'Bulnes 587','cliente');

-- Veamos si se almacenaron en la tabla correspondiente:
select * from empleados;
select * from clientes;

go

-- Creamos un disparador sobre la vista "vista_empleados_clientes" para el evento "update",
-- que redirija las actualizaciones a la tabla correspondiente:
create trigger DIS_empl_clie_actualizar
  on vista_empleados_clientes
  instead of update
  as
   declare @condicion varchar(10)
   set @condicion = (select condicion from inserted)
   if update(documento)
   begin
    raiserror('Los documentos no pueden modificarse', 10, 1)
    rollback transaction
   end
   else
   begin
    if @condicion ='empleado'
    begin
     update empleados set empleados.nombre=inserted.nombre, empleados.domicilio=inserted.domicilio
     from empleados
     join inserted
     on empleados.documento=inserted.documento
    end
    else
     if @condicion ='cliente'
     begin
      update clientes set clientes.nombre=inserted.nombre, clientes.domicilio=inserted.domicilio
      from clientes
      join inserted
      on clientes.documento=inserted.documento
     end
   end;

go

-- Realizamos una actualización sobre la vista, de un empleado:
update vista_empleados_clientes set nombre= 'Ana Maria Acosta' where documento='22222222';

-- Veamos si se actualizó la tabla correspondiente:
select * from empleados;

-- Realizamos una actualización sobre la vista, de un cliente:
update vista_empleados_clientes set domicilio='Bulnes 1234' where documento='23333333';

-- Veamos si se actualizó la tabla correspondiente:
select * from clientes;.
------------------------------------------------------------------------
--APLICACION
-------------------------------------------------------------------------
Un club almacena los datos de sus socios en una tabla denominada "socios", los distintos cursos que 
dictan en "cursos" y las inscripciones de los distintos socios en los distintos cursos en 
"inscriptos".
1- Elimine las tablas si existen:
 if object_id('inscriptos') is not null
  drop table inscriptos;
 if object_id('socios') is not null
  drop table socios;
 if object_id('cursos') is not null
  drop table cursos;

2- Cree las tablas, con las siguientes estructuras:
 create table socios(
  documento char(8) not null,
  nombre varchar(30),
  domicilio varchar(30),
  constraint PK_socios primary key(documento)
 );
 create table cursos(
  numero tinyint identity,
  deporte char(20),
  cantidadmaxima tinyint,
  constraint PK_cursos primary key(numero)
 );

 create table inscriptos(
  documento char(8) not null,
  numerocurso tinyint,
  fecha datetime,
  constraint PK_inscriptos primary key(documento,numerocurso),
  constraint FK_inscriptos_documento
   foreign key (documento)
   references socios(documento),
  constraint FK_inscriptos_curso
   foreign key (numerocurso)
   references cursos(numero)
 );

Los cursos tiene un número que los identifica, y según el deporte, hay un límite de inscriptos (por 
ejemplo, en tenis, no pueden inscribirse más de 4 socios; en natación, solamente se aceptan 6 
alumnos como máximo). Cuando un curso está completo, es decir, hay tantos inscriptos como valor 
tiene el campo "cantidadmaxima"), el socio  queda inscripto en forma condicional. El club guarda esa 
información en una tabla denominada "condicionales" que luego analiza, porque si se inscriben muchos 
para un deporte determinado, se abrirá otro curso.

Un club almacena los datos de sus socios en una tabla denominada "socios", los distintos cursos que 
dictan en "cursos" y las inscripciones de los distintos socios en los distintos cursos en 
"inscriptos".
1- Elimine las tablas si existen:
 if object_id('inscriptos') is not null
  drop table inscriptos;
 if object_id('socios') is not null
  drop table socios;
 if object_id('cursos') is not null
  drop table cursos;

2- Cree las tablas, con las siguientes estructuras:
 create table socios(
  documento char(8) not null,
  nombre varchar(30),
  domicilio varchar(30),
  constraint PK_socios primary key(documento)
 );
 create table cursos(
  numero tinyint identity,
  deporte char(20),
  cantidadmaxima tinyint,
  constraint PK_cursos primary key(numero)
 );

 create table inscriptos(
  documento char(8) not null,
  numerocurso tinyint,
  fecha datetime,
  constraint PK_inscriptos primary key(documento,numerocurso),
  constraint FK_inscriptos_documento
   foreign key (documento)
   references socios(documento),
  constraint FK_inscriptos_curso
   foreign key (numerocurso)
   references cursos(numero)
 );

Los cursos tiene un número que los identifica, y según el deporte, hay un límite de inscriptos (por 
ejemplo, en tenis, no pueden inscribirse más de 4 socios; en natación, solamente se aceptan 6 
alumnos como máximo). Cuando un curso está completo, es decir, hay tantos inscriptos como valor 
tiene el campo "cantidadmaxima"), el socio  queda inscripto en forma condicional. El club guarda esa 
información en una tabla denominada "condicionales" que luego analiza, porque si se inscriben muchos 
para un deporte determinado, se abrirá otro curso.

2- Elimine la tabla "condicionales" si existe:
 if object_id('condicionales') is not null
  drop table condicionales;

3- Cree la tabla, con la siguiente estructura:
 create table condicionales(
  documento char(8) not null,
  codigocurso tinyint not null,
  fecha datetime
 );

4- Ingrese algunos registros en las tablas "socios", "cursos" e "inscriptos":
 insert into socios values('22222222','Ana Acosta','Avellaneda 800');
 insert into socios values('23333333','Bernardo Bustos','Bulnes 345');
 insert into socios values('24444444','Carlos Caseros','Colon 382');
 insert into socios values('25555555','Mariana Morales','Maipu 234');
 insert into socios values('26666666','Patricia Palacios','Paru 587');

 insert into cursos values('tenis',4);
 insert into cursos values('natacion',6);
 insert into cursos values('basquet',20);
 insert into cursos values('futbol',20);

 insert into inscriptos values('22222222',1,getdate());
 insert into inscriptos values('22222222',2,getdate());
 insert into inscriptos values('23333333',1,getdate());
 insert into inscriptos values('23333333',3,getdate());
 insert into inscriptos values('24444444',1,getdate());
 insert into inscriptos values('24444444',4,getdate());
 insert into inscriptos values('25555555',1,getdate());

5- Cree un trigger "instead of" para el evento de inserción para que, al intentar ingresar un 
registro en "inscriptos" controle que el curso no esté completo (tantos inscriptos a tal curso como 
su "cantidadmaxima"); si lo estuviese, debe ingresarse la inscripción en la tabla "condicionales" y 
mostrar un mensaje indicando tal situación. Si la "cantidadmaxima" no se alcanzó, se ingresa la 
inscripción en "inscriptos".

----
select * from cursos
select * from socios
select *from inscriptos;
 select *from condicionales;

----

create trigger dis_inscriptos_insertar
 on inscriptos
 instead of insert
 as
 begin
  declare @maximo tinyint
  select @maximo=cantidadmaxima from cursos as c
                  join inserted as i
                  on c.numero=i.numerocurso
  if (@maximo=(select count(*) from inscriptos as i
               join cursos as c
               on i.numerocurso=c.numero
               join inserted as ins
               on i.numerocurso=ins.numerocurso))
  -- esta completo
  begin
   insert into condicionales select documento,numerocurso,fecha from inserted
   select 'Inscripción condicional.'
  end
  else -- no esta completo
  begin
   insert into inscriptos select documento,numerocurso,fecha from inserted
   select 'Inscripción realizada.'
  end
 end;



6- Inscriba un socio en un curso que no esté completo.
Verifique que el trigger realizó la acción esperada consultando las tablas:

insert into inscriptos values('26666666',2,getdate());
 select *from inscriptos;
 select *from condicionales;

7- Inscriba un socio en un curso que esté completo.
Verifique que el trigger realizó la acción esperada consultando las tablas:

insert into inscriptos values('26666666',1,getdate());
 select *from inscriptos;
 select *from condicionales;


