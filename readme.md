Muchas funciones de SQLite devuelven un entero como código de resultado.   El conjunto mostrado aquí indica éxito o fracaso. en el fururo pueden ser añadidos nuevos codigos.

	
	//--------Codigos extendido de error
	/* En su configuración predeterminada, las rutinas de API SQLite devuelven uno de 30 entero [result codes] .  Sin embargo, la experiencia ha demostrado
 		que muchos de estos códigos de resultado son demasiados imprecisos.  No proveen tanta información acerca de los problemas como a los 
 		programadores les gustaria.  En un esfuerzo para ocuparse de esto, las versiones más nuevas de SQLite (la versión 3.3.8 de fecha : 3.3.8 y más
 		recientes) incluyen soporte para códigos adicionales de resultado que proveen información más detallada acerca de los errores. Estos extended 
 		result codes son habilitado o deshabilitado por la conexión de la base de datos usando sqlite3_extended_result_codes ( )  API. O, puede obtener el 
 		código extendido para el error más reciente usando sqlite3_extended_errcode ( )*/ 
	
	// ------------------------------Banderas de Configuracion y Obciones.-----------------------------------
	
	//-----Obciones de Configuracion-----------------------------------------------------------------------------------------------------

	
	// -------------------Tipos de Datos Fundamentales----------------------------------------------------------------
	// Estas constantes son devueltas por	column_type(). E informan el tipo origuinal de una columna.


proc
	//----------------------Funciones de apertura y Cierre.-------------------------------
	//----------------------Funcion Open.(Abrir)----------------------------------------------------
	// Abre una DB. sin no existe puede crearla, si se le especifica en las banderas de obciones. 
	// Argumento1: Nombre del Archivo  DB, puede contener la ruta
	// Argumento2: UN puntero pasado por referencia, sera actualisado con la direccion de la DB en memoria.
	// Argumento3: Un valor int32 que representa una badera de occion para abrir/ crear el fichero de base de dato [Vea #ObcionesOpen]
	// Argumento4: Es el nombre del objeto sqlite3_vfs que define la interfaz del sistema operativo que la nueva conexión de base de datos debería
	//                       usar. Si el cuarto parámetro es un puntero NULO entonces es usado el objeto sqlite3 _ vfs predeterminado.
	// Si tiene Exito devuelve SQLITE_OK, sino un codigo de error. para informacion de error más detallados llame a extended_errcode() 
	
	
	//--------------------------Funcion Close. (Cerrar)-----------------------------------------------------------------
	// Cierra la DB.
	// Argumento1: Puntero a la DB. Deve ser un puntero valido devuelto por open() o una de la familia
	// Si tiene Exito devuelve SQLITE_OK, sino un codigo de error. para informacion de error más detallados llame a extended_errcode()
		
	//------------------------Funciones para el prosesamiento de errores.------------------------------------
	//errmsg:procedure( ptr_db:SQLite3DB ) {@returns( "eax" ), @stdcall }; external( "_sqlite3_close@4" );
	//extended_errcode()
	
	//-----------------------Funciones utilitarias.---------------------------------------
	//config:procedure( conf:int32 ) {@returns( "eax" ), @stdcall }; external( "sqlite3_config" );
	libversion:procedure {@returns( "eax" ), @stdcall }; external( "_sqlite3_libversion@0" );
	
	
	//---------------------Funciones de ejecucion de centencias SQL.-----------------------------------------
	/* Las sentencias SQL son cadenas de texto, y por lo tanto, las almacenaremos en cadenas.

	Por ejemplo:

	SELECT * FROM usuario;
	INSERT INTO usuario VALUES("Tulana", "tulana@dominio.com");
	DROP usuario;

	No podemos ejecutar las sentencias SQL directamente en nuestros programas. Cada sentencia requiere un proceso, que tendrá diferentes pasos,
	 dependiendo del tipo de sentencia y de cómo decidamos trabajar con ella.

(1) 	El primer paso, es compilar la sentencia. Para ello disponemos de la familia de funciones "prepare", aunque es recomendable usar las que terminan
    	 en v2, dependiendo de si usamos codificación UTF-8 o UTF-16, sqlite3_prepare_v2 o sqlite3_prepare16_v2, respectivamente. El resto de las funciones
    	  se mantienen por compatibilidad, y no deben usarse.
 	*- El primer argumento para estas funciones siempre es un puntero a un objeto de conexión de base de datos válido.
    	*- El segundo es una cadena con la consulta SQL a compilar. La cadena puede contener varias sentencias, separadas con punto y coma.
    	*- El tercer parámetro es la longitud máxima de la cadena que contiene la sentencia, o -1 si el final de la sentencia está marcado con un carácter nulo. si es
    	    0 no se compilara ninguna sentencia. Si el llamador sabe que el string suministrado esta terminado en NULL, entonces hay una pequeña ventaja  de
    	    rendimento pasando un parámetro nByte con el número de bytes en el string incluyendo el terminador NULL
    	*- Para cada sentencia compilada se necesita mantener un objeto de tipo stmt. Las funciones prepare devuelven inicializado el puntero al objeto
    	    referenciado por el cuarto argumento.
    	*- El quinto parámetro se usa cuando la cadena contiene más de una sentencia, 
    	
(2)   	Si la sentencia usa plantillas, el siguiente paso consiste en asignar valores a los parámetros. Si no usa plantillas este paso se omite. Estas asignaciones
    	 se hacen usando la familia de funciones bind*. 
    	 
(3)	A continuación se ejecuta la sentencia, usando la función sqlite3_step. Esta función sólo necesita un parámetro, un puntero a un objeto sqlite3_stmt 
	que contiene los datos de una sentencia compilada. Opcionalmente podemos resetear la sentencia, y volver al punto 2. Esto lo haremos si los valores
	de la plantilla son distintos, por ejemplo, en el caso de sentencias INSERT. O podemos mantener los parámetros y volver al punto 3, por ejemplo, para 
	obtener la siguiente fila de una sentencia SELECT.
	
(4)	Finalmente, borramos la estructura asociada a la sentencia compilada, usando sqlite3_finalice. Esta función también necesita un único parámetro, un
	puntero a un puntero a un objeto sqlite3_stmt. Esta función libera el objeto asociado a una sentencia compilada. */
	
	//----------------------------Paso (1)-------------------------------------------------------------------------------------------------------------------------------------------------------
	//---prepare. Compila a bitecode una sentencia SQL debolviendo en un objeto stmt que fue pasado por referencia. En HLA se maneja como un dword
	//		que apunta a este objeto y es pasado a las diferentes funciones del API SQLite. Use step() para ejecutar una sentencia compilada, o a las
	//		funciones bind* para asignar valores a los parametros de la sentencia SQL si los usa
	//  Devuelve un integer como codigo de error o exito  SQLITE_OK.
	
	
	//-------------------------Paso (2)---------------------------------------------------------------------------------------------------------------------------------------------------------
	// Usar esta familia de funciones para pasar valores a los parametros de una sentencia SQL compilada.
	// En el texto de una sentencia SQL pasada a prepare(), los literales pueden ser reemplazados por un parámetro que corresponde a una de siguientes
	// plantillas.  (?, ?NNN, :VVV, @VVV, $VVV ) NNN representa un entero literal, y VVV representa un identificador alfanumérico. Los valores de estos 
	// parámetros, tambien los parametros por nombre, pueden ser determinados usando bind().
	// El primer argumento para estas funciones es un objeto stmt creado con prepare().
	// El Segundo argumento es el indice del parametro en el conjunto de la sentensia SQL. El más parámetro mas a la izquierda en las sentencia SQL
	// 		tiene el índice 1.  Cuándo el mismo parámetro por nombre es usado más una vez en una sentencia SQL, la segunda y las subsiguientes 
	//		ocurrencias tienen el mismo índice, como la primera ocurrencia. El índice para los parámetros nombrados puede ser visto usando 
	//		bind_parameter_index () si lo desea. El índice para el parametro "?NNN " es el valor NNN. El valor NNN debe estar entre 1 y el limite de parámetro
	//		SQLITE_LIMIT_VARIABLE_NUMBER:   (valor predeterminado 999).
	// El tercer argumento es el valor a ligar al parámetro. Si el tercer parámetro para bind_text () o bind _ text16 () o bind_blob() es un puntero NULL entonses
	//		el cuarto parámetro es ignorado y el resultado final equivale a bind_null().
	// En las rutinas que tienen un cuarto argumento, su valor es el número de bytes en el parámetro. Para ser claro: El valor es el número de bytes en el valor,
	//		 no el número de carácteres. Si el cuarto parámetro para bind_text () o bind _text16 () es negativo, entonces la longitud de la cuerda es el número
	//		de bytes hasta la ocurrencia del promer NULL . Si el cuarto parámetro para bind_blob () es negativo, entonces el comportamiento está indefinido.
	//		Si es pasado un valor positivo a bind_text () o bind _text16 () o bind _text64 () entonces ese parámetro debe ser el byte offset donde el valor de
	//		finalización NUL ocurriría, asumiendo que la cuerda estuviera NUL terminado. Si cualquier carácteres NULL ocurren en los byte offsets menos que
	//		el valor del cuarto parámetro entonces el valor resultante de la cuerda contendrá NULLs incrustados. El resultado de cuerdas que involucran
	//		expresiones con NULLs incrustados está indefinido.
	
	// bind_int 
	//bind_int(stmt:Typstmt, int32, int32);
	
	;
	//--------------------------Paso (3)---------------------------------------------------------------------------------------------------------------------------------------------------------	
	//---step. Ejecuta una sentencia SQL previamente compilada con prepare(). 
	// En una sentencia SELECT  se deve llamar cabes que deseas obtener una fila de la consulta.
	// El código de resultado SQLITE_ROW devuelto por sqlite3 step () señala que otra fila de salida está disponible
	step:procedure( stmt:Typstmt) {@returns( "eax" ), @stdcall }; external( "_sqlite3_step@4" );
	
	//-------------------------Paso ()-----------------------------------------------------------------------------------------------------------------------------------------------------------
	// Para leer los valores devueltos en las colmnas de la fila actual se usan las funciones de las familia column_*()
	// Si la declaración SQL actualmente no señala una fila válida, o si el índice de la columna está fuera de rango, el resultado está indefinido. Estas rutinas 
	//	sólo pueden ser llamadas cuando la llamada más reciente para sqlite3 step () ha devuelto SQLITE_ROW y ni sqlite3 reset () ni sqlite3 finalize () ha
	//	sido llamado posteriormente. Si cualquiera de estas rutinas es llamó después el sqlite3 reset o () sqlite3 finalize o () después de que el sqlite3 step
	//	haya devuelto un valor distinto de SQLITE_ROW, los resultados están indefinidos. Si el sqlite3 step () o sqlite3 reset () o sqlite3 finalize () es
	//	llamado de un hilo diferente mientras cualquiera de estas rutinas está pendiente, luego los resultados están indefinidos.
	// En cada caso el primer argumento es el objeto stmt que está siendo evaluado (stmt  devuelto por prepare()) y 
	// El segundo argumento es el índice de la columna para la cual la información debería ser devuelta. La columna
	//	 más a la izquierda del conjunto de resultado tiene el índice 0. El  número de columnas en el resultado puede
	//	 ser determinado usando column_count ().
	// Las primeras seis rutinas(blob, double, int, int64, texto, y  text16) devuelven el valor de una columna en un formato específico de datos. Si el valor no 
	//	no está inicialmente en el formato que devuelve la turtina entonces se realiza una conversión automática de tipo.(por ejemplo, si consulta devuelve
	//	un entero pero column_text() se usara para extraer el valor este seria convertido)
	
	// column_count. Debuelve un int que representa el nuero de columnas en el objeto stmt.
	
	
	// Recupera los valores integer
	
	
	// Recupera los valores double
	
	
	// Recupera los valores text
	
	
	// Devuelve un valor integer que representa los bytes en un text devuelto por la anterior
	
	
	// Devuelve un valor integer que representa el tipo de dato SQLite de una columna. Ver 
	
	
	// Recupera los valores blob. La mejor froma de definirlo es como un bloque de datos que puede representar cualquier cosa,
	//		un fichero, una imagen....
	
	//-------------------------Paso (4)----------------------------------------------------------------------------------------------------------------------------------------------------------
	//---finalize. Destructor el objeto stmt del que se pasa un puntero. No esta definida su accion para un puntero NULL..
	//  Devuelve un integer como codigo de error o exito  SQLITE_OK.
	
	
	//----------------------Para ejecutar sentencias sencillas, como las de creacion de tablas, en un solo paso------------------------------------------------------------ 
	// Ejecuta una instruccion SQL en un solo paso. esta funcion es una envoltura para las familias anteriores.
	// Argumento1: Puntero a la DB. Deve ser un puntero valido devuelto por open() o una de la familia
	// Argumento2: Una Cadena de caracteres que constitulle la sentencia SQL.
	// Argumento3: Puntero a un procedimento que sera llamado para cada una de las filas devueltas por la sentencia SQL
	// Argumento4: Puntero que sera pasado como primer  argumento a la funcion mensionada antes.
	// Argumento5: Puntero por referencia, sera actualizado a la direccion de una cadena de caracteres que describe el error, o a NULL si n o ocurre
	//			Ningun error. deve liberarse con  sqlite3_free() cuando ya no sea nesesario.
	// Estos tres ultimos arguemntos se pueden pasar como NULL si no se desean usar. 
	// Si tiene Exito devuelve SQLITE_OK, sino un codigo de error. para informacion de error más detallados llame a extended_errcode()
