# Nota entregable ORM versi�n 4.1.0.4
![alt text](image.png)

# Notas:
En este release se crea un nuevo ejecutable con la versi�n 4.1.0.4, lo que indica que todos los clientes que actualmente cuenten con la versi�n 4.1. ... en adelante deben realizar esta actualizaci�n debido a que cubre los ajustes de bugs y novedades presentadas durante la gesti�n de usuarios.

# Bugs corregidos de la versi�n 4.1.0.3
**Nov 12 2015 - Versi�n 4.1.0.3:**   
�	Se ajusta manejo de par�metros de tipo variable de sustituci�n el cual requer�a que el nombre del par�metro en el query estuviera en may�sculas para poder que el remplazo por Regex funcionara (se origin� por ajuste en manejo de nombre de par�metros siempre en may�sculas agregado en 4.1.0.2).

�	Se ajusta el manejo de la cadena de mensaje anexada al query que contiene los datos de los par�metros con los que se lanza la ejecuci�n de la consulta, ya que si este comentario era muy largo el sistema simplemente dejaba de funcionar sin sacar mensaje. Al depurar el c�digo se encontr� que en la clase el comando o DataAdapter.Fill(oDataset); simplemente dejaba de funcionar sin que el sistema capturara la excepci�n. Se recorta la cantidad de informaci�n que se env�a en el comentario.

**Feb 10 2016 - Versi�n 4.1.0.3:**

�	Se ajusta el manejo de "Guardar nuevo reporte" que no estaba funcionando cuando la consulta se encuentra almacenada en la carpeta ra�z (del �rbol de directorios). Se valida que esto funcionaba correctamente en versiones anteriores. Se atribuye el problema a los cambios de funcionalidades de los componentes DevExpress (en particular al TreeList).

�	Se ajusta el proceso de Drag-and-drop para consultas que se encuentren registrados en la carpeta raiz, el sistema no estaba dejando arrastrar dichas consultas para ser ubicadas en una carpeta diferente.

**Feb 11 2016 - Versi�n 4.1.0.3:**

�	Se ajusta el proceso de despacho de correos para que antes de lanzarlo pase la tarea de estado PENDIENTE a EN EJECUCI�N, de esta forma si el servidor de correo se demora en enviar la respuesta de env�o y el timer que recarga las tareas pendientes se cruza con esta ejecuci�n, no se vuelve a cargar esa misma tarea y no despacha varios correos.

**Feb 24 2016 - Versi�n 4.1.0.3:**

�	Se ajusta validaci�n en ```btnCambioClave_ItemClick``` (de la forma ```frmUsuarioAdmon```) para que solo aplique el desencriptado de clave si esta no viene con cadena vac�a.

�	Se cambia el ```this.dsUsuarios.EnforceConstraints = false;``` en la forma para evitar error al realizar b�squedas en Administraci�n de usuarios.

�	Se incluye el tipo 0 - Usuario APL en el combo de tipos de usuario para que dicho tipo no tenga que ser asignado por debajo (script de creaci�n del ORM_APL)    

**Julio 12 2016 - Versi�n 4.1.0.3:**    
�	Al realizar una consulta con una cantidad considerable de campos y hacer uso de la pesta�a �Agrupar�, no se visualizan todos los campos, debido a que se excede el tama�o disponible y no cuenta con una barra de desplazamiento vertical.
Soluci�n: Se incluye un panel con scroll para poder desplazarse verticalmente en caso de que la cantidad de campos supere el tama�o de la pantalla. 

�	Al realizar una consulta con una cantidad considerable de campos y hacer uso de la pesta�a �Ordenar�, no se visualizan todos los campos, debido a que se excede el tama�o disponible y no cuenta con una barra de desplazamiento vertical.
Soluci�n: Se incluye un panel con scroll para poder desplazarse verticalmente en caso de que la cantidad de campos supere el tama�o de la pantalla. 

�	Al configurar una Tarea Programada en el ORM con un usuario diferente al usuario �ORM�, el registro de la tarea NO se visualiza en la ventana que corresponde a "Tareas Programadas� --> �Consultas programadas Activas�. El JOB se ejecuta y genera el archivo, pero NO es posible visualizar las tareas que se tienen programadas y NO permite tener control sobre el JOB que se est�n ejecutando. 
**Soluci�n:** Se ajusta el package ORM_UTILS para que los jobs de ejecuci�n de las consultas programadas sean generados en el esquema ORM y as� los queries que utilizan la vista USER_JOBS puedan ver todos los jobs generados.

# Bugs corregidos para la version 4.1.0.4

**Octubre 4 2016 - Versi�n 4.1.0.4:**

�	Cuando se tiene configurado el Par�metro INICIAR_POST_EXEC_DISPATCHER en 'P' preguntar, cada vez que se inicia el ORM se muestra una ventana la cual pregunta si se desea abrir el Despachador de Tareas, al seleccionar la opci�n 'Si' , este no se abre. 
**Soluci�n:** Se ajusta el procedimiento para que se invoque el m�todo click del bot�n ```btnTareasPostEjec``` y as� tener un solo c�digo de validaci�n de configuraci�n SMTP y apertura de forma de tareas programadas

�	Al acceder a la pesta�a de 'Administraci�n' y seleccionar la opci�n "Consultas"  -->"Roles con Acceso" , se cuenta con los iconos "Refrescar, Asignar permisos, Quitar permisos", estos no funcionan y no realizan ninguna opci�n, ya sea que se encuentre en el modo de Editar o no.
Soluci�n: Se ajusta el llamado a los respectivos procedimientos para refrescar, asignar y quitar permisos de una consulta

�	Al usar la opci�n "Abrir" se despliega la ventana donde se listan las carpetas y consultas, al cambiar una consulta de una carpeta a otra, esta se cambia de manera correcta, pero si se desea mover dentro del �rbol de carpetas despu�s de esta acci�n se genera un Error que indica "Error en la Conexi�n". La conexi�n esta activa, se debe cerrar la ventana y volver abrir.
Soluci�n: Se elimina l�nea en la que se volv�a NULL la conexi�n luego de actualizar el directorio de una consulta arrastrada.

�	Se detecta un escenario en el cual los par�metros definidos dentro de una consulta dejan de reconocerse por el ORM. Esta situaci�n se presenta cuando los par�metros que est�n antecedidos dentro de la misma l�nea por los caracteres ,-   y  estos se encuentran juntos sin ning�n tipo de separador hace que NO sean reconocidos  los par�metros

![alt text](image-1.png)

**Soluci�n:** Se ajusta el error en la clase ```StringTokenizer``` que generaba que un solo signo "-" fuera tomado como inicio de un comentario e invalidara el resto de la l�nea en el query. Se incluye manejo de comentarios multil�neas que no se ten�an en cuenta en el ```StringTokenizer```, lo que podr�a generar errores en al parsear los par�metros.

# Ajustes con la versi�n 4.1.0.4

�	Se incluyen las funciones ```IdentifySession``` en el ```frmMain.cs``` y ```frmConsulta.cs```, con el fin de incluir la informaci�n del nombre del usuario logueado en el campo CLIENT_INFO de V$SESSION ya que un cliente requiere utilizarlo para filtrar listas de valores de par�metros en momentos de ejecuci�n a partir de ese nombre de usuario.      

�	Se incluye tambi�n el manejo de verificar si quien est� cambiando la clave de un usuario es un usuario de tipo administrador (<= 1), para que no le pida la clave actual del usuario y pueda realizar el cambio sin necesidad de conocerla..


