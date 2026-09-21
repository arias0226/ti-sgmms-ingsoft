

<!-- Start of picture text -->
S€Icesi<br><!-- End of picture text -->

### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

## **Diseño de casos funcionales** 

##### **Instrucciones para el Estudiante** 

- El nombre del caso debe describir claramente qué comportamiento se está verificando. 

- Cada caso debe estar asociado a un requerimiento o Historia de Usuario existente 

- El resultado esperado debe ser verificable y específico. No escribir 'Funciona correctamente'. 

- Los pasos deben ser claros, numerados y reproducibles. 

- Incluir casos válidos e inválidos aplicando técnicas de caja negra. 

# **Diseño de casos de prueba funcionales** 

Proyecto: Sistema de Gestión y Monitoreo de Movilidad y Seguridad — SGMMS Curso: Ingeniería de Software II — Tarea Integradora, Entrega 1 Equipo: (Santiago Arias Parra)          Sprint / Iteración: Entrega 1 

### **Escenario base (precondición común a todas las pruebas)** 

Mapa: cuadrícula de 20 x 20. La celda (4, 4) es de tipo OBSTACULO. Flota inicial: PAT-001 PATRULLA (2,3) DISPONIBLE; PAT-002 PATRULLA (15,11) DISPONIBLE; AMB-001 AMBULANCIA (8,14) DISPONIBLE; BOM-001 CAMION_BOMBEROS (19,0) DISPONIBLE; BOM-002 CAMION_BOMBEROS (6,6) FUERA_DE_SERVICIO. Sin incidentes registrados. Puntaje del operador: 0. 

Técnicas de caja negra aplicadas: partición de clases de equivalencia (válidas e inválidas), análisis de valores límite (bordes de la cuadrícula: -1, 0, 19, 20), tabla de decisión (compatibilidad vehículo-incidente) y transición de estados. 

#### **<mark>ID - Nombre Requerimiento/Historia de usuario:  RF1.1 – Registrar incidente</mark>** 

#### Sprint/Iteración:  Entrega 1 

|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|---|---|---|---|---|---|
|CP1.1-01|Válido. Verificar que un<br>incidente con datos válidos se<br>registra con ID consecutivo,<br>estado PENDIENTE y sin vehículo<br>asignado|Sistema iniciado con el<br>escenario base; no hay<br>incidentes registrados|tipo = INCENDIO; fila = 12;<br>columna = 5; gravedad =<br>ALTA; descripción =<br>"Incendio en bodega<br>comercial"|1. Abrir el Panel de<br>Incidentes. 2. Seleccionar<br>"Registrar incidente". 3.<br>Diligenciar tipo, fila, columna,<br>gravedad y descripción. 4.<br>Confirmar|El sistema devuelve el ID INC-0001, mensaje<br>"Incidente INC-0001 registrado correctamente."; el<br>incidente queda con estado PENDIENTE, vehículo<br>"Sin asignar" y fecha/hora de generación igual al<br>instante del registro; el indicador "incidentes<br>activos" pasa de 0 a 1 y el de "incendios activos" de<br>0 a 1|
|CP1.1-02|Válido (valor límite). Verificar<br>que se acepta una ubicación en<br>el borde superior izquierdo de la|Escenario base; existe<br>INC-0001|tipo = ROBO; fila = 0;<br>columna = 0; gravedad =<br>BAJA;descripción =|1. Registrar incidente con fila<br>0 y columna 0. 2. Confirmar.<br>3. Consultar el incidente|El sistema registra el incidente con ID INC-0002 y<br>ubicación (0, 0); el indicador "incidentes activos"<br>pasa a 2yel de "robos activos" a 1|





### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

||cuadrícula<br>i||"Hurto a transeúnte"<br>i   i|creado<br>i|ii|
|---|---|---|---|---|---|
|CP1.1-03|Inválido (valor límite). Verificar<br>que se rechaza una ubicación<br>fuera de la cuadrícula|Escenario base con 2<br>incidentes registrados|tipo = ACCIDENTE; fila =<br>20; columna = 5;<br>gravedad = MEDIA;<br>descripción = "Choque<br>simple"|1. Registrar incidente con fila<br>20. 2. Confirmar|El sistema lanza InvalidLocationException y muestra<br>"La ubicación (20,5) está fuera del mapa."; no se<br>crea ningún incidente, el contador de incidentes<br>activos sigue en 2 y la aplicación continúa<br>ejecutándose|
|CP1.1-04|Inválido. Verificar que se rechaza<br>el registro sobre una celda de<br>tipo obstáculo y con descripción<br>vacía|Escenario base; la celda<br>(4,4) es OBSTACULO|tipo = ROBO; fila = 4;<br>columna = 4; gravedad =<br>ALTA; descripción = ""|1. Registrar incidente en la<br>celda (4,4) con descripción<br>vacía. 2. Confirmar|El sistema lanza InvalidLocationException con el<br>mensaje "La celda (4,4) es un obstáculo y no admite<br>incidentes."; no se crea el incidente, ninguna<br>estructura se modifica y los indicadores permanecen<br>iguales|



|**ID - Nom**<br>Sprint/Ite|**bre Requerimiento/Historia d**<br>ración:  Entrega 1|**e usuario:  RF1.2 – Con**|**sultar incidentepor ide**|**ntificador**||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP1.2-01|**i**<br>Válido. Verificar que la consulta<br>por ID existente devuelve la ficha<br>completa del incidente|Existe INC-0001<br>(INCENDIO, (12,5), ALTA,<br>PENDIENTE, sin vehículo)|idIncidente = "INC-0001"|1. Abrir el Panel de<br>Incidentes. 2. Digitar<br>INC-0001 en "Buscar por ID".<br>3. Ejecutar la búsqueda|El sistema muestra: ID INC-0001, tipo INCENDIO,<br>ubicación (12, 5), gravedad ALTA, fecha/hora en<br>formato dd-MM-yyyy HH:mm:ss, descripción, estado<br>PENDIENTE, vehículo "Sin asignar" y fecha de<br>resolución "-". Ninguna estructura ni indicador<br>cambia|
|CP1.2-02|Inválido. Verificar que la consulta<br>por un ID inexistente informa el<br>error sin cerrar la aplicación|Existen INC-0001 e<br>INC-0002; no existe<br>INC-9999|idIncidente = "INC-9999"|1. Digitar INC-9999 en<br>"Buscar por ID". 2. Ejecutar la<br>búsqueda|El sistema lanza IncidentNotFoundException y<br>muestra "No existe un incidente con el identificador<br>INC-9999."; no se muestra ficha, la aplicación sigue<br>activayel estado del sistema no cambia|



|**ID - Nom**|**bre Requerimiento/Historia d**|**e usuario:  RF1.3 – List**|**ar incidentes activos**|||
|---|---|---|---|---|---|
|Sprint/Ite|ración:  Entrega 1|||||
|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP1.3-01|**i**<br>Válido. Verificar que el listado<br>filtrado por tipo devuelve<br>únicamente los incidentes<br>activos de ese tipo|Existen INC-0001<br>(INCENDIO, PENDIENTE),<br>INC-0002 (ROBO,<br>PENDIENTE) e INC-0009<br>(ROBO,EN_PROCESO)|filtroTipo = ROBO;<br>filtroEstado = null|1. Abrir el Panel de<br>Incidentes. 2. Seleccionar el<br>filtro de tipo ROBO. 3. Aplicar<br>el filtro|La lista muestra exactamente 2 filas: INC-0002 e<br>INC-0009, cada una con ID, tipo, ubicación,<br>gravedad, estado y vehículo; totalIncidentesActivos<br>= 2; INC-0001 no aparece y ninguna estructura se<br>modifica|
|CP1.3-02|Válido (clase vacía). Verificar que<br>el sistema informa<br>correctamente cuando ningún<br>incidente cumple el filtro|Todos los incidentes<br>registrados están en<br>estado RESUELTO|filtroTipo = null;<br>filtroEstado = null|1. Abrir el Panel de<br>Incidentes. 2. Consultar la<br>lista de incidentes activos sin<br>filtros|i<br>El sistema devuelve una lista vacía,<br>totalIncidentesActivos = 0 y muestra el mensaje "No<br>hay incidentes activos."; no se lanza ninguna<br>excepciónyla aplicación continúa|
|CP1.3-03|i<br>Inválido (clase de equivalencia<br>fuera de dominio). Verificar que<br>el listado de activos rechaza el<br>filtro de estado RESUELTO|Existen INC-0001 e<br>INC-0002 activos y al<br>menos un incidente<br>RESUELTO|filtroTipo = null;<br>filtroEstado = RESUELTO|i<br>1. Abrir el Panel de<br>Incidentes. 2. Seleccionar el<br>filtro de estado RESUELTO. 3.<br>Aplicar el filtro|i<br>El sistema lanza InvalidFilterException y muestra "El<br>listado de incidentes activos no admite el filtro de<br>estado RESUELTO."; no se devuelve listado alguno, la<br>vista conserva el resultado anterior y la aplicación<br>sigue operativa|





### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

|**ID - Nombre Requerimiento/Historia de usuario:  RF1.4 – Actualizar el estado de un incidente**|
|---|



|Sprint/Ite|ración:  Entrega 1|||||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP1.4-01|Válido (con bonificación).<br>Verificar que resolver un<br>incidente ALTA dentro del<br>tiempo máximo otorga 120<br>puntos y libera el vehículo|INC-0001 (ALTA) está<br>EN_PROCESO con<br>BOM-001 asignado; han<br>transcurrido 45 s de<br>simulación desde su<br>generación (< 60 s);<br>puntaje del operador = 0|idIncidente = "INC-0001";<br>nuevoEstado = RESUELTO|1. Seleccionar INC-0001 en el<br>Panel de Incidentes. 2. Pulsar<br>"Resolver incidente". 3.<br>Confirmar|El incidente queda RESUELTO con fecha/hora de<br>resolución registrada; puntajeObtenido = 120 (100<br>base + 20 bonificación) y puntajeTotalOperador =<br>120; BOM-001 queda DISPONIBLE sin incidente<br>asignado; el incidente desaparece del árbol, de la<br>cola de prioridad y del listado de activos, pero sigue<br>consultable por ID; los indicadores "incidentes<br>activos"y"vehículos disponibles" se actualizan|
|CP1.4-02|Válido (sin bonificación).<br>Verificar que resolver un<br>incidente MEDIA después del<br>tiempo máximo otorga solo el<br>puntaje base|INC-0004 (ACCIDENTE,<br>MEDIA) está<br>EN_PROCESO con<br>AMB-001; han<br>transcurrido 150 s de<br>simulación (> 120 s);<br>puntaje del operador =<br>120|idIncidente = "INC-0004";<br>nuevoEstado = RESUELTO|1. Seleccionar INC-0004. 2.<br>Pulsar "Resolver incidente".<br>3. Confirmar|puntajeObtenido = 70 (sin bonificación) y<br>puntajeTotalOperador = 190; el mensaje indica que<br>no se otorgó bonificación por exceder el tiempo<br>máximo; AMB-001 queda DISPONIBLE|
|CP1.4-03|Inválido (transición de estados).<br>Verificar que no se permite<br>resolver un incidente que está<br>PENDIENTE y sin vehículo<br>asignado<br>i|INC-0002 está PENDIENTE<br>y sin vehículo asignado|idIncidente = "INC-0002";<br>nuevoEstado = RESUELTO|1. Seleccionar INC-0002. 2.<br>Pulsar "Resolver incidente".<br>3. Confirmar|El sistema lanza InvalidStatusTransitionException<br>con el mensaje "No se puede resolver un incidente<br>PENDIENTE sin atención previa."; INC-0002 sigue<br>PENDIENTE, el puntaje del operador no cambia y<br>ninguna estructura se modifica<br>i|
|CP1.4-04|Válido (valor límite del tiempo<br>de bonificación). Verificar que un<br>incidente BAJA resuelto justo<br>antes de los 180 s recibe la<br>bonificación|INC-0005 (ROBO, BAJA)<br>está EN_PROCESO con<br>PAT-001; han transcurrido<br>170 s de simulación (<<br>180 s); puntaje del<br>operador = 190|idIncidente = "INC-0005";<br>nuevoEstado = RESUELTO|1. Seleccionar INC-0005. 2.<br>Pulsar "Resolver incidente".<br>3. Confirmar|puntajeObtenido = 60 (40 base + 20 bonificación) y<br>puntajeTotalOperador = 250; PAT-001 queda<br>DISPONIBLE sin incidente asignado; el incidente sale<br>del árbol y de la cola de prioridad pero sigue<br>consultable por ID|



|**ID - Nom**<br>Sprint/Ite|**bre Requerimiento/Historia d**<br>ración:  Entrega 1<br>**i**|**e usuario:  RF2.1 – Con**|**sultar el incidente de m**|**ayorprioridad**||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**<br>i|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP2.1-01|Válido (orden por gravedad).<br>Verificar que el sistema devuelve<br>el incidente de gravedad ALTA<br>aunque no sea el más reciente ni<br>elprimero registrado|Activos: INC-0010 (BAJA,<br>14:00:00), INC-0011<br>(MEDIA, 14:01:00),<br>INC-0012 (ALTA,<br>14:02:00)<br>i|— (la operación no recibe<br>parámetros)|1. Abrir el Panel de<br>Incidentes. 2. Consultar<br>"Incidente de mayor<br>prioridad"|El sistema muestra INC-0012 con gravedad ALTA, su<br>tipo, ubicación y fecha/hora; el incidente no se retira<br>de la cola de prioridad ni del árbol: al repetir la<br>consulta se obtiene el mismo resultado y el total de<br>activos sigue en 3|
|CP2.1-02|Válido (desempate por<br>antigüedad). Verificar que ante<br>dos incidentes de igual gravedad<br>se devuelve el más antiguo|Activos: INC-0013 (ALTA,<br>14:05:00) e INC-0014<br>(ALTA, 14:03:00)|—|1. Consultar "Incidente de<br>mayor prioridad"|El sistema muestra INC-0014 (14:03:00) por ser el<br>más antiguo entre los de gravedad ALTA, aplicando<br>el criterio de desempate R6; el resultado coincide<br>con el máximo del árbol binario de búsqueda|





### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

|CP2.1-03|Inválido (estructura vacía).<br>i|No hay incidentes activos|—<br>1. Consultar "Incidente de|La operación de la estructura lanza<br>i|
|---|---|---|---|---|
||Verificar que la consulta sobre<br>una cola de prioridad vacía se<br>maneja sin cerrar la aplicación|(todos RESUELTO o<br>ninguno registrado)|mayor prioridad"|EmptyStructureException, la aplicación la captura y<br>muestra "No hay incidentes activos."; la ventana<br>sigue operativayel estado del sistema no cambia|



|**ID - Nom**<br>Sprint/Ite<br>|**bre Requerimiento/Historia**<br>ración:  Entrega 1<br> **i**|**de usuario:  RF2.2 – Aten**<br>|**der el incidente de m**<br>|**ayorprioridad**<br>||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**<br>i|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP2.2-01|Válido. Verificar que "Atender<br>siguiente" extrae el incidente<br>más prioritario y sugiere un<br>vehículo compatible<br>i|Activos: INC-0003<br>(ACCIDENTE, ALTA) e<br>INC-0011 (ROBO, MEDIA);<br>AMB-001 DISPONIBLE<br>i|—|1. Pulsar "Atender siguiente"<br>en el Panel de Incidentes|El sistema pone INC-0003 como incidente en<br>atención y sugiere AMB-001; INC-0003 se retira de la<br>cola de prioridad, del árbol y de la cola de despacho,<br>pero sigue consultable por ID (RF1.2); el total de<br>pendientes de asignación disminuye en 1; una nueva<br>consulta de mayor prioridad devuelve ahora<br>INC-0011<br>i|
|CP2.2-02|Inválido. Verificar el<br>comportamiento al pulsar<br>"Atender siguiente" sin<br>incidentes activos|No hay incidentes activos|—|1. Pulsar "Atender siguiente"|Se lanza EmptyStructureException, capturada por la<br>interfaz, que muestra "No hay incidentes activos.";<br>no se selecciona ningún incidente ni se sugiere<br>vehículo alguno; las estructuras permanecen vacías y<br>la aplicación continúa|



#### **<mark>ID - Nombre Requerimiento/Historia de usuario:  RF2.3 – Listar incidentes act</mark> i** **<mark>vos ordenados por prioridad</mark>** 

|Sprint/Ite<br>|ración:  Entrega 1<br> **i**|||||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i i|**Precondición**<br>i|**Datos de Entrada**<br>i|**Pasos**|**Resultado Esperado**|
|CP2.3-01|Válido (sin filtro). Verificar que el<br>listado ordena por gravedad<br>descendente y, dentro de cada<br>gravedad, por antigüedad<br>ascendente|Activos: INC-0010 (BAJA,<br>14:00), INC-0011 (MEDIA,<br>14:01), INC-0012 (ALTA,<br>14:02), INC-0014 (ALTA,<br>14:03)|filtroGravedad = null|1. Abrir el Panel de<br>Incidentes. 2. Seleccionar la<br>vista "Ordenados por<br>prioridad" sin filtro|La lista devuelve exactamente el orden INC-0012,<br>INC-0014, INC-0011, INC-0010; el primer elemento<br>coincide con el resultado de RF2.1; el árbol y la cola<br>de prioridad no se modifican y el total de activos<br>sigue en 4|
|CP2.3-02|Válido (clase vacía). Verificar el<br>listado ordenado cuando no hay<br>incidentes activos|No hay incidentes activos|filtroGravedad = null<br>i|1. Seleccionar la vista<br>"Ordenados por prioridad"|El sistema devuelve una lista vacía con<br>totalIncidentesActivos = 0 y el mensaje "No hay<br>incidentes activos.",sin lanzar excepción<br>i|
|CP2.3-03|Inválido (clase de equivalencia<br>fuera de dominio). Verificar que<br>se rechaza un filtro de gravedad<br>que no pertenece al dominio|Existe al menos INC-0012<br>activo|filtroGravedad =<br>"CRITICA" (valor<br>inexistente en Severity)|1. Seleccionar la vista<br>"Ordenados por prioridad".<br>2. Introducir la gravedad<br>CRITICA como filtro. 3.<br>Aplicar|El sistema lanza InvalidFilterException y muestra<br>"Gravedad de filtro no válida: use ALTA, MEDIA o<br>BAJA."; no se devuelve listado, el árbol y la cola de<br>prioridad no se modifican y la aplicación sigue<br>operativa|



#### **<mark>ID - Nombre Requerimiento/Historia de usuario:  RF3.1 – Registrar vehículo de atención</mark>** 

|Sprint/It|eración:  Entrega 1<br>**i**|||||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**|**Datos de Entrada**|**Pasos**<br>i|**Resultado Esperado**|
|CP3.1-01|Válido. Verificar que un vehículo|Escenario base: 4|idVehiculo = "AMB-002";<br>ii|1. Abrir la gestión de|El sistema registra AMB-002 con estado DISPONIBLE,|
||nuevo se registra DISPONIBLEy|vehículos disponibles|tipo = AMBULANCIA;fila =|vehículos. 2. Seleccionar|sin incidente asignadoyubicación(9,14);muestra|





### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

||actualiza el indicador de<br>disponibles<br>i|(PAT-001, PAT-002,<br>AMB-001, BOM-001)|9; columna = 14|"Registrar vehículo". 3.<br>Diligenciar ID, tipo y<br>ubicación. 4. Confirmar|"Vehículo AMB-002 registrado en (9, 14)."; el<br>indicador "vehículos disponibles" pasa de 4 a 5 y<br>AMB-002 es recuperablepor RF3.2<br>i|
|---|---|---|---|---|---|
|CP3.1-02|Inválido. Verificar que se rechaza<br>el registro de un vehículo con<br>identificador ya existente|Escenario base: PAT-001<br>ya está registrado|idVehiculo = "PAT-001";<br>tipo = PATRULLA; fila =<br>10; columna = 10|1. Seleccionar "Registrar<br>vehículo". 2. Diligenciar el ID<br>PAT-001. 3. Confirmar|El sistema lanza DuplicatedIdException con el<br>mensaje "Ya existe un vehículo con el identificador<br>PAT-001."; no se crea un segundo vehículo, la<br>ubicación de PAT-001 sigue en (2, 3) y el indicador<br>de disponibles no cambia|



|**ID - Nom**<br>Sprint/Ite|**bre Requerimiento/Historia d**<br>ración:  Entrega 1|**e usuario:  RF3.2 – Con**|**sultar vehículopor ide**|**ntificador**||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP3.2-01|**i**<br>Válido. Verificar que la consulta<br>devuelve la ficha completa del<br>vehículo, incluido el incidente<br>asignado|AMB-001 está EN_RUTA<br>atendiendo INC-0003|idVehiculo = "AMB-001"|1. Abrir la gestión de<br>vehículos. 2. Digitar AMB-001<br>en "Buscar por ID". 3.<br>Ejecutar la búsqueda|El sistema muestra: ID AMB-001, tipo AMBULANCIA,<br>ubicación (8, 14), estado EN_RUTA e incidente<br>asignado INC-0003; ninguna estructura ni indicador<br>cambia|
|CP3.2-02|Inválido. Verificar que la consulta<br>de un vehículo inexistente<br>informa el error sin cerrar la<br>aplicación|No existe el vehículo<br>AMB-099|idVehiculo = "AMB-099"|1. Digitar AMB-099 en<br>"Buscar por ID". 2. Ejecutar la<br>búsqueda|El sistema lanza VehicleNotFoundException y<br>muestra "No existe un vehículo con el identificador<br>AMB-099."; no se muestra ficha y la aplicación sigue<br>operativa|



|**ID - Nom**|**bre Requerimiento/Historia d**|**e usuario:  RF3.3 – Lista**|**r vehículospor estado**|||
|---|---|---|---|---|---|
|Sprint/Ite|ración:  Entrega 1<br>**i**|||||
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i   i|**Precondición**|**Datos de Entrada**<br>i|**Pasos**<br>i|**Resultado Esperado**<br>i|
|CP3.3-01|Válido. Verificar que el filtro por<br>estado DISPONIBLE coincide con<br>el indicador del Centro de<br>Monitoreo<br>i|Escenario base: PAT-001,<br>PAT-002, AMB-001 y<br>BOM-001 DISPONIBLE;<br>BOM-002<br>FUERA_DE_SERVICIO|filtroEstado =<br>DISPONIBLE; filtroTipo =<br>null<br>i|1. Abrir la gestión de<br>vehículos. 2. Aplicar el filtro<br>de estado DISPONIBLE<br>i|La lista muestra exactamente 4 filas (PAT-001,<br>PAT-002, AMB-001, BOM-001) con ID, tipo,<br>ubicación, estado e incidente;<br>totalVehiculosFiltrados = 4, igual al indicador<br>"vehículos disponibles" del Centro de Monitoreo;<br>BOM-002 no aparece|
|CP3.3-02|Válido (clase vacía). Verificar el<br>comportamiento cuando ningún<br>vehículo cumple la combinación<br>de filtros|Escenario base; ninguna<br>ambulancia está<br>FUERA_DE_SERVICIO|filtroEstado =<br>FUERA_DE_SERVICIO;<br>filtroTipo = AMBULANCIA<br>i|1. Aplicar los filtros estado<br>FUERA_DE_SERVICIO y tipo<br>AMBULANCIA<br>i|El sistema devuelve una lista vacía con<br>totalVehiculosFiltrados = 0 y el mensaje "No hay<br>vehículos que cumplan el filtro.", sin lanzar<br>excepción<br>i|
|CP3.3-03|Inválido (clase de equivalencia<br>fuera de dominio). Verificar que<br>se rechaza un estado de filtro<br>inexistente|Escenario base con los 5<br>vehículos registrados|filtroEstado =<br>"EN_MANTENIMIENTO"<br>(valor inexistente en<br>VehicleStatus); filtroTipo<br>= null|1. Abrir la gestión de<br>vehículos. 2. Introducir<br>EN_MANTENIMIENTO como<br>filtro de estado. 3. Aplicar|El sistema lanza InvalidFilterException y muestra<br>"Estado de filtro no válido: EN_MANTENIMIENTO no<br>es un estado de vehículo."; no se devuelve listado y<br>el registro de vehículos no se modifica|
|**ID - Nom**|**bre Requerimiento/Historia d**|**e usuario:  RF3.4 – Actu**|**alizar el estado de un v**|**ehículo**||
|Sprint/Ite|ración:  Entrega 1<br>**i**|||||
|**ID Caso**|**Nombre Caso(Objetivo)**|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|





### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

|CP3.4-01|Válido (transición de estados).<br>Verificar que un vehículo<br>EN_RUTA puede pasar a<br>ATENDIENDO al llegar al<br>incidente|PAT-002 está EN_RUTA<br>asignado a INC-0002;<br>vehículos disponibles = 3<br>(PAT-001, AMB-001,<br>BOM-001)|idVehiculo = "PAT-002";<br>nuevoEstado =<br>ATENDIENDO|1. Seleccionar PAT-002 en la<br>gestión de vehículos. 2.<br>Cambiar el estado a<br>ATENDIENDO. 3. Confirmar|El vehículo queda ATENDIENDO y se muestra<br>"Vehículo PAT-002 pasó de EN_RUTA a<br>ATENDIENDO."; el indicador "vehículos disponibles"<br>permanece en 3, porque el vehículo ya no estaba<br>disponible antes del cambio<br>ii|
|---|---|---|---|---|---|
|CP3.4-02|Inválido (transición de estados).<br>Verificar que se rechaza poner<br>FUERA_DE_SERVICIO un vehículo<br>que tiene un incidente asignado|AMB-001 está<br>ATENDIENDO el incidente<br>INC-0003|idVehiculo = "AMB-001";<br>nuevoEstado =<br>FUERA_DE_SERVICIO|1. Seleccionar AMB-001. 2.<br>Cambiar el estado a<br>FUERA_DE_SERVICIO. 3.<br>Confirmar|El sistema lanza InvalidStatusTransitionException<br>con el mensaje "AMB-001 tiene asignado INC-0003:<br>libere el vehículo antes de retirarlo de servicio.";<br>AMB-001 sigue ATENDIENDO con su incidente y los<br>indicadores no cambian|



|**ID - Nom**<br>Sprint/Ite<br>|**bre Requerimiento/Historia d**<br>ración:  Entrega 1<br> **i**|**e usuario:  RF4.1 – Pro**<br>|**poner vehículo candida**<br>|**to**<br>||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP4.1-01|Válido. Verificar que el sistema<br>propone el vehículo más<br>específico entre los compatibles<br>y disponibles, sin asignarlo<br>i|INC-0003 es ACCIDENTE,<br>PENDIENTE; AMB-001 y<br>PAT-001 están<br>DISPONIBLE|idIncidente = "INC-0003"|1. Seleccionar INC-0003 en el<br>Panel de Incidentes. 2. Pulsar<br>"Sugerir vehículo"|El sistema propone AMB-001 (ambulancia antes que<br>patrulla para un accidente) y muestra "Vehículo<br>sugerido para INC-0003: AMB-001 en (8, 14)."; no se<br>realiza ninguna asignación: INC-0003 sigue<br>PENDIENTE sin vehículo y AMB-001 sigue<br>DISPONIBLE sin incidente<br>ii|
|CP4.1-02|Inválido. Verificar el<br>comportamiento cuando no<br>existe ningún vehículo disponible<br>y compatible|INC-0007 es INCENDIO,<br>PENDIENTE; BOM-001<br>está ATENDIENDO y<br>BOM-002 está<br>FUERA_DE_SERVICIO;<br>solo quedan patrullas y<br>ambulancias disponibles|idIncidente = "INC-0007"|1. Seleccionar INC-0007. 2.<br>Pulsar "Sugerir vehículo"|El sistema lanza NoCompatibleVehicleException y<br>muestra "No hay vehículos disponibles compatibles<br>con INCENDIO."; no se sugiere ni se asigna vehículo<br>alguno y el estado del sistema no cambia|



|**ID - Nom**<br>Sprint/Ite|**bre Requerimiento/Historia d**<br>ración:  Entrega 1<br>**i**|**e usuario:  RF4.2 – Asig**|**nar vehículo a un incide**|**nte**||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP4.2-01|Válido. Verificar que una<br>asignación compatible y<br>disponible enlaza incidente y<br>vehículo y actualiza ambos<br>estados|INC-0007 es INCENDIO,<br>PENDIENTE, sin vehículo;<br>BOM-001 está<br>DISPONIBLE; vehículos<br>disponibles = 4|idIncidente = "INC-0007";<br>idVehiculo = "BOM-001"|1. Seleccionar INC-0007. 2.<br>Elegir BOM-001 en la lista de<br>vehículos. 3. Pulsar "Asignar<br>vehículo". 4. Confirmar|El sistema muestra "BOM-001 asignada a INC-0007.<br>Incidente EN_PROCESO."; INC-0007 queda<br>EN_PROCESO con vehículo BOM-001; BOM-001<br>queda EN_RUTA con incidente INC-0007; el<br>incidente sale de la cola de despacho; "vehículos<br>disponibles" pasa de 4 a 3 y "última acción"<br>devuelve ASSIGN INC-0007 / BOM-001<br>ii|
|CP4.2-02|Inválido (tabla de decisión —<br>incompatible). Verificar que una<br>patrulla no puede ser asignada a<br>un incendio|INC-0007 es INCENDIO,<br>PENDIENTE; PAT-001 está<br>DISPONIBLE|idIncidente = "INC-0007";<br>idVehiculo = "PAT-001"|1. Seleccionar INC-0007. 2.<br>Elegir PAT-001. 3. Pulsar<br>"Asignar vehículo". 4.<br>Confirmar|El sistema lanza IncompatibleVehicleException con<br>el mensaje "Una PATRULLA no puede atender un<br>INCENDIO."; INC-0007 sigue PENDIENTE sin vehículo,<br>PAT-001 sigue DISPONIBLE sin incidente, el indicador<br>de disponibles no cambia y la pila de acciones no<br>registra nada|





<!-- Start of picture text -->
S€Icesi<br><!-- End of picture text -->

### **Universidad Icesi** 

### **Departamento de Computación y Sistemas Inteligentes Ingeniería de Software II** 

|CP4.2-03|Inválido (no disponible). Verificar<br>que un vehículo compatible pero<br>ocupado no puede ser asignado|INC-0008 es INCENDIO,<br>PENDIENTE; BOM-002 es<br>compatible pero está<br>FUERA_DE_SERVICIO|idIncidente = "INC-0008";<br>idVehiculo = "BOM-002"|1. Seleccionar INC-0008. 2.<br>Elegir BOM-002. 3. Pulsar<br>"Asignar vehículo". 4.<br>Confirmar|El sistema lanza VehicleNotAvailableException con el<br>mensaje "El vehículo BOM-002 no está disponible<br>(FUERA_DE_SERVICIO)."; ni el incidente ni el<br>vehículo cambian de estado<br>i|
|---|---|---|---|---|---|
|CP4.2-04|Inválido (incidente resuelto).<br>Verificar que un incidente<br>RESUELTO no puede recibir<br>vehículos<br>ii|INC-0001 está RESUELTO;<br>PAT-002 está DISPONIBLE|idIncidente = "INC-0001";<br>idVehiculo = "PAT-002"|1. Buscar INC-0001 por ID. 2.<br>Elegir PAT-002. 3. Pulsar<br>"Asignar vehículo"|El sistema lanza InvalidAssignmentException con el<br>mensaje "El incidente INC-0001 ya está resuelto y no<br>admite asignaciones."; PAT-002 sigue DISPONIBLE y<br>elpuntaje del operador no cambia<br>i|
|CP4.2-05|Inválido (identificador<br>inexistente). Verificar que se<br>valida primero la existencia del<br>incidente|No existe INC-9999;<br>AMB-001 está<br>DISPONIBLE|idIncidente = "INC-9999";<br>idVehiculo = "AMB-001"|1. Digitar INC-9999 como<br>incidente. 2. Elegir AMB-001.<br>3. Pulsar "Asignar vehículo"|El sistema lanza IncidentNotFoundException con el<br>mensaje "No existe un incidente con el identificador<br>INC-9999." antes de validar el vehículo; AMB-001<br>permanece DISPONIBLE y la aplicación sigue<br>operativa|



#### **<mark>ID - Nombre Requerimiento/Historia de usuario:  RF4.3 – Liberar vehículo al f</mark> i** **<mark>nalizar la atención</mark>** 

|Sprint/Ite|ración:  Entrega 1<br>**i**|||||
|---|---|---|---|---|---|
|**ID Caso**|**Nombre Caso(Objetivo)**<br>i|**Precondición**|**Datos de Entrada**|**Pasos**|**Resultado Esperado**|
|CP4.3-01|Válido (cancelación). Verificar<br>que al cancelar una asignación el<br>vehículo queda disponible y el<br>incidente vuelve a la cola de<br>despacho<br>i|INC-0003 está<br>EN_PROCESO con<br>AMB-001 EN_RUTA;<br>vehículos disponibles = 3|idIncidente = "INC-0003";<br>esCancelacion = true|1. Seleccionar INC-0003. 2.<br>Pulsar "Cancelar asignación".<br>3. Confirmar|AMB-001 queda DISPONIBLE, sin incidente asignado<br>y ubicada en (7, 9) (celda del incidente INC-0003);<br>INC-0003 vuelve a PENDIENTE sin vehículo y se<br>reinserta al final de la cola de despacho; "vehículos<br>disponibles" pasa de 3 a 4; el mensaje muestra<br>"AMB-001 liberadaydisponible en(7,9)."<br>i|
|CP4.3-02|Inválido. Verificar que no se<br>puede liberar un vehículo de un<br>incidente que no tiene<br>asignación|INC-0002 está PENDIENTE<br>y sin vehículo asignado|idIncidente = "INC-0002";<br>esCancelacion = true|1. Seleccionar INC-0002. 2.<br>Pulsar "Cancelar asignación"|El sistema lanza InvalidAssignmentException con el<br>mensaje "El incidente INC-0002 no tiene ningún<br>vehículo asignado."; el incidente sigue PENDIENTE,<br>ningún vehículo cambia de estado y el indicador de<br>disponibles no varía|



