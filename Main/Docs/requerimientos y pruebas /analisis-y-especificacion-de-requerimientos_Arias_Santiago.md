### **Análisis y especificación del problema y los requerimientos funcionales** 

|Cliente|Alcaldia Municipal de Palmira (Valle del Cauca) - Secretarias de Movilidad y de Seguridad<br>Ciudadana.|
|---|---|
|Usuario|Operador del Centro de Monitoreo Urbano: funcionario de turno encargado de vigilar el estado<br>de movilidad y seguridad de la ciudad en un mapa 2D, registrar y consultar los incidentes activos,<br>decidir que vehiculo de atencion despachar a cada incidente y cerrar la atencion cuando el<br>incidente queda resuelto. Es el unico rol del sistema en esta entrega.|
|Contexto del problema|Palmira ha crecido en poblacion, expansion urbana y circulacion vehicular, lo que produjo<br>congestion en las vias principales durante las horas pico e incidentes de transito, robos e<br>incendios que requieren atencion oportuna. Hoy la ciudad NO cuenta con una herramienta<br>centralizada que integre el monitoreo de incidentes, las rutas, los vehiculos disponibles y la<br>capacidad de respuesta de las unidades de emergencia, lo que dificulta la toma de decisiones<br>rapidas.<br>El SGMMS es un prototipo de escritorio en Java con interfaz grafica que simula el<br>comportamiento de la ciudad sobre un mapa 2D simplificado. El operador observa en el Centro<br>de Monitoreo los indicadores globales (incidentes activos por tipo, vehiculos disponibles y<br>puntaje), navega el Mapa de Trafico con las teclas W/A/S/D o flechas, y desde el Panel de<br>Incidentes registra, consulta, prioriza y atiende los incidentes. Los robos y los incendios se<br>generan de forma aleatoria por zona; los accidentes se generan unicamente a partir del<br>comportamiento de la simulacion, segun criterios definidos y justificados por el equipo.<br>Cada incidente tiene un tipo (accidente, robo o incendio), una gravedad (alta, media o baja) y un<br>estado (pendiente, en proceso o resuelto). Los incidentes activos se organizan en un arbol binario<br>de busqueda ordenado por gravedad, de mayor a menor prioridad, con la fecha y hora de<br>generacion como criterio de desempate. El operador asigna vehiculos (patrulla, ambulancia o<br>camion de bomberos) validando la compatibilidad con el tipo de incidente y la disponibilidad del<br>vehiculo; toda asignacion invalida se informa mediante una excepcion personalizada y no<br>modifica el estado del sistema. Al resolver correctamente un incidente el operador gana puntos<br>segun la gravedad, con bonificacion si la atencion se completo dentro del tiempo maximo<br>definido para ese nivel.<br>Restricciones no explicitas que el equipo asume: el prototipo no usa bases de datos, internet,<br>GPS, APIs externas ni mapas reales; la configuracion inicial (mapa, rutas y vehiculos) se carga<br>desde archivos JSON validados, y el estado de la simulacion se guarda y recupera mediante<br>serializacion de objetos Java; no se permite usar ArrayList, LinkedList, HashMap, TreeMap,<br>PriorityQueue, Stack ni ArrayDeque de Java: todas las estructuras de datos son propias y<br>genericas, ubicadas en el paquete model.structures. La aplicacion es monousuario y se ejecuta en<br>una sola maquina.|
|Requerimientos<br>funcionales (listado)|RF1 - Gestionar incidentes<br>RF1.1 - Registrar incidente<br>RF1.2 - Consultar incidente por identificador<br>RF1.3 - Listar incidentes activos<br>RF1.4 - Actualizar el estado de un incidente<br>RF2 - Gestionar la prioridad de los incidentes<br>RF2.1 - Consultar el incidente de mayor prioridad|



||RF2.2 - Atender el incidente de mayor prioridad<br>RF2.3 - Listar incidentes activos ordenados por prioridad<br>RF3 - Gestionar vehiculos de atencion<br>RF3.1 - Registrar vehiculo de atencion<br>RF3.2 - Consultar vehiculo por identificador<br>RF3.3 - Listar vehiculos por estado<br>RF3.4 - Actualizar el estado de un vehiculo<br>RF4 - Asignar vehiculos a incidentes<br>RF4.1 - Proponer vehiculo candidato<br>RF4.2 - Asignar vehiculo a un incidente<br>RF4.3 - Liberar vehiculo al finalizar la atencion|
|---|---|
|Requerimientos no<br>funcionales (listado)|RNF1 - El sistema debe desarrollarse en Java y ejecutarse correctamente en la version definida<br>para el curso (JDK 17 o superior), sin dependencias de servicios externos.<br>RNF2 - El sistema debe ofrecer una interfaz grafica clara y consistente entre el Centro de<br>Monitoreo, el Mapa de Trafico y el Panel de Incidentes.<br>RNF3 - El sistema debe informar con un mensaje claro y especifico el resultado de toda operacion,<br>exitosa o rechazada.<br>RNF4 - El sistema debe controlar los errores de ejecucion mediante excepciones personalizadas,<br>evitando cierres inesperados y preservando el estado consistente del modelo.<br>RNF5 - El sistema debe mantener la consistencia entre todas las estructuras que gestionan un<br>mismo elemento (indice hash, BST, cola de prioridad y cola de despacho).<br>RNF6 - Las consultas por identificador deben resolverse en O(1) esperado; la consulta del<br>incidente de mayor prioridad en O(1) y su extraccion en O(log n).<br>RNF7 - Todas las estructuras de datos deben ser implementadas por el equipo, de forma generica,<br>en el paquete model.structures.<br>RNF8 - El codigo debe escribirse en ingles, organizarse en paquetes y mantener la separacion del<br>patron MVC.<br>RNF9 - El sistema debe recuperar de manera consistente el estado de una simulacion<br>previamente almacenada.<br>RNF10 - El prototipo no debe usar bases de datos, internet, GPS, APIs externas ni mapas reales.|
||RP1 - El proyecto se desarrolla en equipos de 3 estudiantes sobre un repositorio Git creado desde<br>GitHub Classroom.|
|Requerimientos de<br>proceso (listado)|RP2 - El repositorio sigue el estandar Gitflow: main como linea base actualizada por Pull Request<br>desde develop, develop para integrar y ramas feat/<nombre> por funcionalidad.<br>RP3 - El repositorio debe contener README.md, la carpeta doc/ con la documentacion en<br>markdown, el diagrama UML (PDF y fuentes .vpp) y un .gitignore.|
||RP4 - El equipo debe reportar indicadores de calidad en 15 commits equi-temporales,|





referenciando sus SHA en el README. RP5 - Todo requerimiento funcional y toda estructura de datos debe contar con pruebas unitarias en JUnit. RP6 - Los tres entregables de esta entrega deben ser consistentes entre si antes de subirse al repositorio. 

# **Especificacion de los requerimientos funcionales** 

## **RF1.1 - Registrar incidente** 

|Identificador y nombre|RF1.1 - Registrar incidente||
|---|---|---|
|Resumen|El sistema permite al opera      i<br>incendio. Recibe el tipo, la<br>automáticamente el identifi i<br>generación, y fija el estado<br>valida que el tipo y la grave<br>la cuadrícula y no sea un ob<br>falla lanza la excepción corr<br>exitosa inserta el incidente<br>gravedad, en la cola de prio     i<br>identificador asignado junt   i|dor registrar un nuevo incidente de tipo accidente, robo o<br>i  ubicación en el mapa, la gravedad y una descripción; genera<br>i  iicador consecutivo con formato INC-#### y la fecha y hora de<br>i   inicial en PENDIENTE sin vehículo asignado. Antes de registrar<br>i   dad pertenezcan a sus dominios, que la ubicación esté dentro de<br>stáculo y que la descripción no esté vacía; si alguna condición<br>espondiente y no crea el incidente. Cuando la validación es<br>en el índice hash por ID, en el árbol binario de búsqueda por<br>ridad de atención y al final de la cola de despacho, y devuelve el<br>ii  o con la confirmación del registro.|
|Entradas|**Nombre entrada**|**Tipo de dato**<br>**Condicion valores validos**|
||tipoIncidente|IncidentType<br>Debe ser uno de ACCIDENTE,<br>ROBO, INCENDIO. No nulo.|
||fila|int<br>Entero en [0, FILAS-1] (0 a 19<br>en la configuración inicial).|
||columna|int<br>Entero en [0, COLUMNAS-1]<br>(0 a 19 en la configuración<br>inicial).|
||gravedad|Severity<br>Debe ser uno de ALTA,<br>MEDIA, BAJA. No nulo.|
||descripcion|String<br>Cadena no nula, sin espacios<br>al inicio/fin, de 1 a 200<br>caracteres.|
|Resultado o<br>Postcondicion|Existe en el sistema un nue   ii<br>PENDIENTE, sin vehículo asi<br>registro. El incidente queda<br>de búsqueda por gravedad,<br>contador de incidentes acti     i<br>validación falla, el sistema n        i|vo incidente con identificador único INC-####, estado<br>gnado y con fecha y hora de generación igual al instante del<br>simultáneamente presente en el índice hash, en el árbol binario<br>en la cola de prioridad de atención y en la cola de despacho. El<br>ivos y el contador del tipo correspondiente aumentan en 1. Si la<br>o crea el incidente y ninguna estructura se modifica.|
|Salidas|**Nombre salida**|**Tipo de dato**<br>**Formato**|
||idIncidente|String<br>INC-#### (p. ej. INC-0007)|
||fechaHoraGeneracion|LocalDateTime<br>dd-MM-yyyy HH:mm:ss|
||mensajeConfirmacion|String<br>"Incidente INC-0007<br>registrado correctamente."|
||mensajeError|String<br>Texto descriptivo de la<br>excepción, p. ej. "La<br>ubicación (25,3) está fuera<br>del mapa."|



## **RF1.2 - Consultar incidente por identificador** 

|Identificador y nombre|RF1.2 - Consultar incidente  ii|por identificador||
|---|---|---|---|
|Resumen|El sistema permite al opera         i<br>identificador. Recibe el ID, l<br>esperado y, si existe, devue  i<br>generación, la descripción,<br>resolución (si el incidente y     ii<br>IncidentNotFoundExceptio<br>no modifica el estado del si|dor consultar toda la inform     i<br>ii    o busca en el índice HashTab<br>lve el tipo, la ubicación, la gr<br>el estado, el vehículo asigna<br>a fue resuelto). Si el identific<br>in y la interfaz informa el erro<br>i    stema.|ación de un incidente a partir de su<br>ii         le<String, Incident> en O(1)<br>i    avedad, la fecha y hora de<br>do (si lo hay) y la fecha y hora de<br>iiador no existe en el índice, lanza<br>i      r sin cerrar la aplicación. La consulta|
|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
||idIncidente|String|No nulo ni vacío; formato<br>INC-#### y debe existir en el<br>índice de incidentes.|
|Resultado o<br>Postcondicion|El sistema devuelve la infor<br>estructura de datos ni los in   ii<br>mediante IncidentNotFouni|mación completa del inciden<br>dicadores. Si el identificador<br>dException y el estado del sis|te solicitado sin alterar ninguna<br>ii no existe, se informa el error<br>i     tema permanece intacto.|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||idIncidente|String|INC-####|
||tipoIncidente|IncidentType|ACCIDENTE | ROBO |<br>INCENDIO|
||ubicacion|String|(fila, columna) - p. ej. (12, 5)|
||gravedad|Severity|ALTA | MEDIA | BAJA|
||fechaHoraGeneracion|LocalDateTime|dd-MM-yyyy HH:mm:ss|
||descripcion|String|Texto libre|
||estado|IncidentStatus|PENDIENTE | EN_PROCESO |<br>RESUELTO|
||idVehiculoAsignado|String|AAA-### o "Sin asignar"|
||fechaHoraResolucion|LocalDateTime|dd-MM-yyyy HH:mm:ss o "-"<br>si no está resuelto|



## **RF1.3 - Listar incidentes activos** 

|Identificador y nombre|RF1.3 - Listar incidentes activos|
|---|---|
|Resumen|El sistema permite al operador visualizar en el Panel de Incidentes la lista de los incidentes<br>activos, entendidos como aquellos cuyo estado es PENDIENTE o EN_PROCESO.<br>Opcionalmente recibe un filtro por tipo de incidente y/o por estado; si no se indica filtro<br>devuelve todos los activos. Para cada incidente muestra el identificador,el tipo,la ubicación,|





la gravedad, el estado y el vehículo asignado. Si no existen incidentes que cumplan el criterio, devuelve una lista vacía y la interfaz muestra el mensaje "No hay incidentes activos." sin lanzar excepción. La operación no modifica el estado del sistema. 

|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
|---|---|---|---|
||filtroTipo|IncidentType|Opcional. Si se indica debe<br>ser ACCIDENTE, ROBO o<br>INCENDIO; null significa<br>todos los tipos.|
||filtroEstado|IncidentStatus|Opcional. Solo se aceptan<br>PENDIENTE o EN_PROCESO;<br>null significa ambos.|
|Resultado o<br>Postcondicion|El sistema devuelve la colec   i    i  i<br>ninguna estructura de dato<br>error.|ción de incidentes activos que   i  i<br>s. La lista puede estar vacía, lo|i  cumplen el filtro, sin modificar<br>cual es un resultado válido y no un|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||listaIncidentesActivos|LinkedList<Incident>|Filas con ID | Tipo |<br>Ubicación | Gravedad |<br>Estado | Vehículo|
||totalIncidentesActivos|int|Entero >= 0|
||mensajeInformativo|String|"No hay incidentes activos."<br>cuando la lista es vacía|



## **RF1.4 - Actualizar el estado de un incidente** 

|Identificador y nombre|RF1.4 - Actualizar el esta|do de un incidente|
|---|---|---|
|Resumen|El sistema permite al ope   i<br>estado. Recibe el identifi        i<br>incidente exista y que la<br>Para pasar a EN_PROCES<br>RESUELTO el sistema reg<br>según la gravedad y el tie<br>incidente del árbol binar            i<br>libera el vehículo asignad<br>IncidentNotFoundExcepi      i<br>InvalidStatusTransitionExi   i|rador iniciar o finalizar la atención de un incidente actualizando su<br>iicador del incidente y el nuevo estado solicitado, verifica que el<br>transición sea válida (PENDIENTE -> EN_PROCESO -> RESUELTO).<br>O el incidente debe tener un vehículo asignado. Al pasar a<br>istra la fecha y hora de resolución, calcula el puntaje obtenido<br>impo empleado, lo suma al puntaje del operador, elimina el<br>io de búsqueda, de la cola de prioridad y del índice de activos, y<br>o dejándolo en estado DISPONIBLE. Si el incidente no existe lanza<br>tion; si la transición no está permitida lanza<br>iception y no modifica nada.|
|Entradas|**Nombre entrada**|**Tipo de dato**<br>**Condicion valores validos**|
||idIncidente|String<br>Formato INC-####; debe<br>existir en el índice de<br>incidentes.|
||nuevoEstado|IncidentStatus<br>EN_PROCESO o RESUELTO.<br>La transición debe ser válida<br>(R9).|
|Resultado o|El incidentequeda con e|l nuevo estadoytodas las estructuras reflejan el cambio de forma|



|Postcondicion|consistente. Si el nuevo est        i i<br>hora de resolución, el punta       i<br>asignado queda DISPONIBL         i<br>vehículos disponibles se act<br>sistema no realiza ningún c|ado es RESUELTO: el inciden    i i<br>je del operador aumenta en   i<br>E y sin incidente, y los indica   i<br>ualizan. Si la transición es in<br>ambio.|te deja de estar activo, tiene fecha y<br>base + bonificación, el vehículo<br>dores de incidentes activos y<br>válida o el incidente no existe, el|
|---|---|---|---|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||estadoActualizado|IncidentStatus|EN_PROCESO | RESUELTO|
||puntajeObtenido|int|Entero en {0, 40, 60, 70, 90,<br>100, 120}|
||puntajeTotalOperador|int|Entero >= 0|
||fechaHoraResolucion|LocalDateTime|dd-MM-yyyy HH:mm:ss|
||mensajeConfirmacion|String|"Incidente INC-0007<br>resuelto. +120 puntos (100<br>base + 20 bonificación)."|



## **RF2.1 - Consultar el incidente de mayor prioridad** 

Identificador y nombre RF2.1 - Consultar el incidente de mayor prioridad Resumen El sistema permite al operador conocer cuál es el incidente activo más prioritario sin retirarlo de la estructura. La prioridad se determina por la gravedad (ALTA > MEDIA > BAJA) y, ante igual gravedad, por la menor fecha y hora de generación. La consulta se resuelve leyendo el tope de la cola de prioridad (peekMax) en O(1), y el resultado debe coincidir con el máximo del árbol binario de búsqueda, que aplica el mismo comparador. Devuelve el identificador, el tipo, la gravedad, la ubicación, la fecha y hora de generación y el estado del incidente. Si no hay incidentes activos, la interfaz informa "No hay incidentes activos." y la operación de la estructura lanza EmptyStructureException, que se maneja sin cerrar la aplicación. La consulta no modifica el estado del sistema. 

|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
|---|---|---|---|
||(sin parámetros)|-|Precondición: debe existir al<br>menos un incidente activo<br>en la cola de prioridad.|
|Resultado o<br>Postcondicion|El sistema devuelve el incid i<br>el árbol binario de búsqued<br>activos se informa la situac|ente activo de mayor priori<br>a y los indicadores permane<br>i    ión y el estado del sistema p|i   dad sin extraerlo: la cola de prioridad,<br>cen sin cambios. Si no hay incidentes<br>i          ermanece intacto.|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||idIncidentePrioritario|String|INC-####|
||gravedad|Severity|ALTA | MEDIA | BAJA|
||tipoIncidente|IncidentType|ACCIDENTE | ROBO |<br>INCENDIO|



|ubicacion|String|(fila, columna)|
|---|---|---|
|fechaHoraGeneracion|LocalDateTime|dd-MM-yyyy HH:mm:ss|
|mensajeInformativo|String|"No hay incidentes activos."<br>cuando la estructura está<br>vacía|



## **RF2.2 - Atender el incidente de mayor prioridad** 

|Identificador y nombre|RF2.2 - Atender el incident|e de mayor prioridad||
|---|---|---|---|
|Resumen|El sistema permite al opera  i<br>activo más prioritario. Extr<br>aplicando el criterio de gra    i<br>en foco del Panel de Incide<br>extraído se elimina tambié<br>mantener la consistencia e<br>resuelto. Si no existen incid i  i<br>informa la situación sin cer|dor seleccionar automáticam<br>i   ae el incidente del tope de la<br>vedad y desempate por antig<br>ntes y sugiere el vehículo can<br>n del árbol binario de búsque<br>ntre estructuras, pero perma<br>entes activos, lanza EmptySti<br>rar la aplicación.|iente para atención el incidente<br>i          cola de prioridad en O(log n)<br>iüedad, lo marca como el incidente<br>didato correspondiente. El incidente<br>da y de la cola de despacho para<br>nece en el índice hash hasta que sea<br>i  ructureException y la interfaz|
|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
||(sin parámetros)|-|Precondición: la cola de<br>prioridad no está vacía.|
|Resultado o<br>Postcondicion|El incidente de mayor prior<br>Incidentes y retirado de la<br>despacho; sigue accesible p<br>asignación disminuye en 1.     i    i|idad queda seleccionado co<br>i   cola de prioridad, del árbol bi<br>or su ID en el índice hash. El<br>Si no hay incidentes activos,    i|mo incidente en atención del Panel de<br>i        nario de búsqueda y de la cola de<br>número de incidentes pendientes de<br>i ninguna estructura se modifica.|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||incidenteEnAtencion|Incident|Ficha completa del incidente<br>en el Panel de Incidentes|
||vehiculoSugerido|String|AAA-### o "Sin vehículo<br>compatible disponible"|
||mensajeConfirmacion|String|"Atendiendo INC-0007<br>(ALTA, generado 20-09-2026<br>14:03:11)."|



## **RF2.3 - Listar incidentes activos ordenados por prioridad** 

Identificador y nombre RF2.3 - Listar incidentes activos ordenados por prioridad Resumen El sistema permite al operador visualizar todos los incidentes activos ordenados de mayor a menor prioridad. El listado se obtiene mediante un recorrido en inorden inverso del árbol binario de búsqueda de incidentes, de modo que los incidentes de gravedad ALTA aparecen primero y, dentro de cada nivel de gravedad, los más antiguos antes que los más recientes. El resultado permite al operador verificar el orden de atención y debe ser coherente con el incidente devuelto por RF2.1, <u>que corresponde siempre al primer elemento de esta lista. Si</u> 

||no hay incidentes activos dev       i<br>correspondiente, sin lanzar e|i uelve una lista vacía con el me i<br>xcepción.|i       nsaje informativo|
|---|---|---|---|
|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
||(sin parámetros)|-|La operación no recibe<br>parámetros.|
|Resultado o<br>Postcondicion|El sistema devuelve la lista de  i<br>modificar el árbol binario de<br>elemento de la lista coincide|incidentes activos ordenada<br>i     búsqueda, la cola de prioridad<br>con el resultado de RF2.1.|i  por prioridad descendente, sin<br>i          ni los indicadores. El primer|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||listaOrdenadaPorPrioridad|LinkedList<Incident>|Filas Posición | ID |<br>Gravedad | Fecha/Hora |<br>Tipo | Estado|
||totalIncidentesActivos|int|Entero >= 0|
||mensajeInformativo|String|"No hay incidentes activos."<br>cuando la lista es vacía|



## **RF3.1 - Registrar vehículo de atención** 

|Identificador y nombre|RF3.1 - Registrar vehículo|de atención|
|---|---|---|
|Resumen<br>|El sistema permite regist       l<br>configuración inicial desd<br>Recibe el identificador, e i<br>identificador cumpla el f    i<br>que la ubicación esté den<br>correcta crea el vehículo<br>la tabla hash de vehículo<br>DuplicatedIdException o ii<br>|rar un vehículo de atención en la flota, ya sea al cargar la<br>i  e el archivo JSON o cuando el operador lo agrega manualmente.<br>ii l tipo de vehículo y su ubicación inicial en el mapa. Valida que el<br>ii   ormato AAA-### acorde al tipo y que no exista ya en el registro, y<br>tro de la cuadrícula y no sea un obstáculo. Si la validación es<br>con estado DISPONIBLE y sin incidente asignado, y lo almacena en<br>s con el ID como clave; en caso contrario lanza<br>i  InvalidLocationException y no registra nada.<br> <br>|
|Entradas|**Nombre entrada**|**Tipo de dato**<br>**Condicion valores validos**|
||idVehiculo|String<br>Formato AAA-### con AAA<br>en {PAT, AMB, BOM}<br>coherente con el tipo; no<br>debe existir en el registro.|
||tipoVehiculo|VehicleType<br>PATRULLA, AMBULANCIA o<br>CAMION_BOMBEROS. No<br>nulo.|
||fila|int<br>Entero en [0, FILAS-1].|
||columna|int<br>Entero en [0, COLUMNAS-1].|
|Resultado o<br>Postcondicion|El vehículo queda registr<br>incidente asignado y ubic<br>aumenta en 1. Si el ID es<br>estado del sistema no ca|ado en la tabla hash de vehículos con estado DISPONIBLE, sin<br>ado en la celda indicada. El indicador de vehículos disponibles<br>tá duplicado o la ubicación es inválida, no se registra el vehículo y el<br>mbia.|



|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
|---|---|---|---|
||idVehiculo|String|AAA-### (p. ej. AMB-002)|
||estadoInicial|VehicleStatus|DISPONIBLE|
||totalVehiculosDisponibles|int|Entero >= 0|
||mensajeConfirmacion|String|"Vehículo AMB-002<br>registrado en (8, 14)."|



## **RF3.2 - Consultar vehículo por identificador** 

|Identificador y nombre|RF3.2 - Consultar vehículo  ii|por identificador||
|---|---|---|---|
|Resumen|El sistema permite al oper        i<br>identificador. Recibe el ID,<br>devuelve el tipo, la ubicaci          i<br>identificador no existe lan i<br>cerrar la aplicación. La con  i|ador consultar la información     i<br>ii    lo busca en la tabla hash de<br>i  ón actual, el estado y el incid    i<br>ii   za VehicleNotFoundExceptio<br>sulta no modifica el estado d|de un vehículo a partir de su<br>ii           vehículos en O(1) esperado y<br>i        ente asignado si lo tiene. Si el<br>ii    in y la interfaz informa el error sin<br>i   el sistema.|
|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
||idVehiculo|String|No nulo ni vacío, con<br>formato AAA-###, y debe<br>existir en el registro de<br>vehículos.|
|Resultado o<br>Postcondicion|El sistema devuelve la info     i<br>el identificador no existe s  i<br>permanece intacto.|rmación completa del vehícu  i<br>ii   e informa VehicleNotFoundEi|lo sin modificar ninguna estructura. Si<br>ii     xception y el estado del sistema|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||idVehiculo|String|AAA-###|
||tipoVehiculo|VehicleType|PATRULLA | AMBULANCIA |<br>CAMION_BOMBEROS|
||ubicacion|String|(fila, columna)|
||estado|VehicleStatus|DISPONIBLE | EN_RUTA |<br>ATENDIENDO |<br>FUERA_DE_SERVICIO|
||idIncidenteAsignado|String|INC-#### o "Sin asignar"|



## **RF3.3 - Listar vehículos por estado** 

|Identificador y nombre|RF3.3 - Listar vehículos por estado|
|---|---|
|Resumen|El sistema permite al operador consultar los vehículos de la flota, opcionalmente filtrados<br>por estadoy/opor tipo, para saber conqué recursos cuenta antes de asignar. Recorre la|





tabla hash de vehículos y devuelve, para cada vehículo que cumple el filtro, su identificador, tipo, ubicación, estado e incidente asignado. Cuando el filtro es DISPONIBLE, el número de elementos devueltos debe coincidir con el indicador "vehículos disponibles" del Centro de Monitoreo. Si ningún vehículo cumple el criterio devuelve una lista vacía con un mensaje informativo, sin lanzar excepción, y no modifica el estado del sistema. 

|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
|---|---|---|---|
||filtroEstado|VehicleStatus|Opcional; DISPONIBLE,<br>EN_RUTA, ATENDIENDO o<br>FUERA_DE_SERVICIO. null<br>significa todos.|
||filtroTipo|VehicleType|Opcional; PATRULLA,<br>AMBULANCIA o<br>CAMION_BOMBEROS. null<br>significa todos.|
|Resultado o<br>Postcondicion|El sistema devuelve la lista d     i     i<br>ninguna estructura ni indica|e vehículos que cumplen el fil     i<br>dor. La lista vacía es un resulta|itro y su conteo, sin modificar<br>do válido.|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||listaVehiculos|LinkedList<Vehicle>|Filas ID | Tipo | Ubicación |<br>Estado | Incidente|
||totalVehiculosFiltrados|int|Entero >= 0|
||mensajeInformativo|String|"No hay vehículos que<br>cumplan el filtro." cuando la<br>lista es vacía|



## **RF3.4 - Actualizar el estado de un vehículo** 

|Identificador y nombre|RF3.4 - Actualizar el esta|do de un vehículo|
|---|---|---|
|Resumen|El sistema controla los ca<br>un incidente o por decisi     ii<br>estado, verifica que el ve<br>-> ATENDIENDO -> DISPO<br>y con retorno únicament<br>pasar a FUERA_DE_SERV<br>VehicleNotFoundExcepti      i<br>InvalidStatusTransitionExi<br>disponibilidad recalcula e|mbios de estado de un vehículo producidos durante la atención de<br>ón del operador. Recibe el identificador del vehículo y el nuevo<br>i   hículo exista y que la transición sea válida (DISPONIBLE -> EN_RUTA<br>NIBLE, con FUERA_DE_SERVICIO alcanzable desde cualquier estado<br>e a DISPONIBLE). Un vehículo con incidente asignado no puede<br>ICIO sin antes ser liberado. Si el vehículo no existe lanza<br>ion; si la transición no está permitida lanza<br>iception y el estado no cambia. Toda actualización que afecte la<br>l indicador de vehículos disponibles.|
|Entradas|**Nombre entrada**|**Tipo de dato**<br>**Condicion valores validos**|
||idVehiculo|String<br>Formato AAA-###; debe<br>existir en el registro de<br>vehículos.|
||nuevoEstado|VehicleStatus<br>DISPONIBLE, EN_RUTA,<br>ATENDIENDO o<br>FUERA_DE_SERVICIO;la|



||||transición desde el estado<br>actual debe ser válida.|
|---|---|---|---|
|Resultado o<br>Postcondicion|El vehículo queda con el nuev<br>Monitoreo refleja el cambio.<br>vehículo y el indicador perma|o estado y el indicador "veh<br>l   Si la transición es inválida o<br>necen sin cambios.|ículos disponibles" del Centro de<br>l         el vehículo no existe, el estado del|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||estadoActualizado|VehicleStatus|DISPONIBLE | EN_RUTA |<br>ATENDIENDO |<br>FUERA_DE_SERVICIO|
||totalVehiculosDisponibles|int|Entero >= 0|
||mensajeConfirmacion|String|"Vehículo PAT-003 pasó de<br>EN_RUTA a ATENDIENDO."|
||mensajeError|String|"AMB-001 tiene asignado<br>INC-0003: libere el vehículo<br>antes de retirarlo de<br>servicio."|



## **RF4.1 - Proponer vehículo candidato** 

|Identificador y nombre|RF4.1 - Proponer vehículo c|andidato|
|---|---|---|
|Resumen|El sistema propone automái<br>incidente, como sugerencia     i   ii<br>incidente, verifica que exist<br>prioridad con los vehículos<br>DISPONIBLE y ser compatibl   i<br>es el tipo de vehículo más ei<br>accidentes) y, ante empate,   ii        i<br>con la distancia calculada so<br>no existe ningún vehículo di  i  ii<br>interfaz informa la situación       i<br>sistema.|ticamente al operador el vehículo más adecuado para atender un<br>que el operador debe confirmar. Recibe el identificador del<br>i  a y que su estado sea PENDIENTE, y construye una cola de<br>que cumplen simultáneamente dos condiciones: estar en estado<br>ies con el tipo de incidente. El criterio de orden en esta entrega<br>i    specífico para el incidente (ambulancia antes que patrulla para<br>el menor identificador; en la Entrega 3 este criterio se refinará<br>bre el grafo del mapa. Devuelve el vehículo del tope de la cola. Si<br>sponible y compatible lanza NoCompatibleVehicleException y la<br>sin asignar nada. La operación no modifica el estado del|
|Entradas|**Nombre entrada**|**Tipo de dato**<br>**Condicion valores validos**|
||idIncidente|String<br>Formato INC-####; debe<br>existir y su estado debe ser<br>PENDIENTE.|
|Resultado o<br>Postcondicion|El sistema muestra en el Pa<br>realizar ninguna asignación:<br>candidato, se informa la situ|nel de Incidentes el vehículo sugerido para el incidente, sin<br>ni el incidente ni el vehículo cambian de estado. Si no existe<br>ación y el sistema queda igual.|
|Salidas|**Nombre salida**|**Tipo de dato**<br>**Formato**|
||idVehiculoCandidato|String<br>AAA-###|
||tipoVehiculoCandidato|VehicleType<br>PATRULLA | AMBULANCIA |<br>CAMION_BOMBEROS|



|ubicacionVehiculo|String|(fila, columna)|
|---|---|---|
|mensajeSugerencia|String|"Vehículo sugerido para<br>INC-0007: AMB-001 en (8,<br>14)."|
|mensajeError|String|"No hay vehículos<br>disponibles compatibles con<br>INCENDIO."|



## **RF4.2 - Asignar vehículo a un incidente** 

|Identificador y nombre|RF4.2 - Asignar vehículo a un|incidente||
|---|---|---|---|
|Resumen|El sistema permite al operad         i<br>el identificador del incidente<br>(IncidentNotFoundException     i<br>incidente no esté RESUELTO<br>(InvalidAssignmentException   i    i   i<br>incidente (IncompatibleVehii<br>(VehicleNotAvailableExcepti<br>el incidente en ambos sentid<br>EN_PROCESO, retira el incide<br>registra la acción en la pila d<br>validación fallida rechaza la o   i<br>vehículo ni de los indicadore|or asignar un vehículo de a    i<br>ii   y el del vehículo, y valida e<br>i), que el vehículo exista (Vei<br>ni ya EN_PROCESO con veh<br>i), que el tipo de vehículo s i   i<br>icleException) y que el vehíc<br>ion). Si todas las validacione<br>ios, cambia el estado del ve<br>i  nte de la cola de despacho<br>e acciones del operador y a<br>peración completa sin moi<br>s.|tención a un incidente activo. Recibe<br>ii         n orden: que el incidente exista<br>i     hicleNotFoundException), que el<br>ículo asignado<br>i   i   ea compatible con el tipo de<br>ii    ulo esté DISPONIBLE<br>i    s se cumplen, enlaza el vehículo con<br>i     hículo a EN_RUTA y el del incidente a<br>i        de pendientes de asignación,<br>ctualiza los indicadores. Cualquier<br>dificar el estado del incidente, del|
|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
||idIncidente|String|Formato INC-####; debe<br>existir y su estado debe ser<br>PENDIENTE.|
||idVehiculo|String|Formato AAA-###; debe<br>existir, ser compatible con el<br>tipo del incidente y estar<br>DISPONIBLE.|
|Resultado o<br>Postcondicion|El incidente queda en estado<br>asignado; el vehículo queda<br>asignado; el incidente sale d<br>disminuye en 1 y la acción qu<br>como "última acción". Si algu<br>ningún elemento del sistema|EN_PROCESO con el vehíc<br>en estado EN_RUTA con el<br>e la cola de despacho; el in<br>eda en el tope de la pila de<br>i   na validación falla, se lanza<br>cambia de estado.|ulo registrado como vehículo<br>incidente registrado como incidente<br>dicador de vehículos disponibles<br>acciones del operador, consultable<br>i        la excepción correspondiente y|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||estadoIncidente|IncidentStatus|EN_PROCESO|
||estadoVehiculo|VehicleStatus|EN_RUTA|
||totalVehiculosDisponibles|int|Entero >= 0|
||mensajeConfirmacion|String|"AMB-001 asignada a<br>INC-0007. Incidente|



|||EN_PROCESO."|
|---|---|---|
|mensajeError|String|"Una PATRULLA no puede<br>atender un INCENDIO." / "El<br>vehículo BOM-002 no está<br>disponible<br>(FUERA_DE_SERVICIO)."|



## **RF4.3 - Liberar vehículo al finalizar la atención** 

|Identificador y nombre|RF4.3 - Liberar vehículo al finalizar la atención|
|---|---|
|Resumen|El sistema libera el vehículo que atendía un incidente cuando la atención finaliza, de modo<br>que vuelva a estar disponible para nuevas asignaciones. Recibe el identificador del incidente,<br>verifica que exista y que tenga un vehículo asignado; si el incidente no tiene vehículo lanza<br>InvalidAssignmentException. Al liberar, rompe el enlace bidireccional entre incidente y<br>vehículo, cambia el estado del vehículo a DISPONIBLE, deja su ubicación en la celda del<br>incidente atendido y actualiza el indicador de vehículos disponibles. Esta operación se<br>ejecuta automáticamente como parte de RF1.4 cuando el incidente pasa a RESUELTO, y<br>también puede invocarse cuando el operador cancela una asignación, caso en el cual el<br>incidente regresa a estado PENDIENTE y vuelve a encolarse en la cola de despacho.|



|Entradas|**Nombre entrada**|**Tipo de dato**|**Condicion valores validos**|
|---|---|---|---|
||idIncidente|String|Formato INC-####; debe<br>existir y tener un vehículo<br>asignado.|
||esCancelacion|boolean|true si el operador cancela la<br>asignación; false si la<br>liberación se produce por<br>resolución del incidente.|
|Resultado o<br>Postcondicion|El vehículo queda en estado<br>incidente atendido; el indicad<br>true, el incidente vuelve a PE        i<br>de despacho. Si esCancelacio<br>incidente no existe o no tenía|DISPONIBLE, sin incidente a<br>or de vehículos disponible<br>NDIENTE, sin vehículo asig     i<br>n = false, el incidente cons<br>vehículo, no se realiza nin|signado y ubicado en la celda del<br>s aumenta en 1. Si esCancelacion =<br>nado, y se reinserta al final de la cola<br>erva el estado RESUELTO. Si el<br>gún cambio.|
|Salidas|**Nombre salida**|**Tipo de dato**|**Formato**|
||idVehiculoLiberado|String|AAA-###|
||estadoVehiculo|VehicleStatus|DISPONIBLE|
||estadoIncidente|IncidentStatus|RESUELTO o PENDIENTE|
||totalVehiculosDisponibles|int|Entero >= 0|
||mensajeConfirmacion|String|"AMB-001 liberada y<br>disponible en (12, 5)."|



