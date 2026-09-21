# Diseño de casos de prueba funcionales

**Proyecto:** Sistema de Gestión y Monitoreo de Movilidad y Seguridad — SGMMS
**Curso:** Ingeniería de Software II — Tarea Integradora, Entrega 1
**Equipo:** _(integrante 1, integrante 2, integrante 3)_
**Sprint / Iteración:** Entrega 1

---

## Escenario base (precondición común a todas las pruebas)

Salvo que un caso indique lo contrario, el sistema se inicia con esta configuración cargada desde el JSON inicial:

**Mapa:** cuadrícula de 20 × 20. La celda `(4, 4)` es de tipo `OBSTACULO`.

**Flota inicial**

| ID | Tipo | Ubicación | Estado |
|---|---|---|---|
| `PAT-001` | `PATRULLA` | (2, 3) | `DISPONIBLE` |
| `PAT-002` | `PATRULLA` | (15, 11) | `DISPONIBLE` |
| `AMB-001` | `AMBULANCIA` | (8, 14) | `DISPONIBLE` |
| `BOM-001` | `CAMION_BOMBEROS` | (19, 0) | `DISPONIBLE` |
| `BOM-002` | `CAMION_BOMBEROS` | (6, 6) | `FUERA_DE_SERVICIO` |

**Sin incidentes registrados. Puntaje del operador: 0.**

**Técnicas de caja negra aplicadas:** partición de clases de equivalencia (válidas e inválidas), análisis de valores límite (bordes de la cuadrícula: −1, 0, 19, 20), tabla de decisión (compatibilidad vehículo–incidente, R4) y transición de estados (R9).

---

## RF1 — Gestionar incidentes

### CP de RF1.1

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF1.1 – Registrar incidente |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP1.1-01 | **Válido.** Verificar que un incidente con datos válidos se registra con ID consecutivo, estado `PENDIENTE` y sin vehículo asignado | Sistema iniciado con el escenario base; no hay incidentes registrados | tipo = `INCENDIO`; fila = 12; columna = 5; gravedad = `ALTA`; descripción = `"Incendio en bodega comercial"` | 1. Abrir el Panel de Incidentes. 2. Seleccionar "Registrar incidente". 3. Diligenciar tipo, fila, columna, gravedad y descripción. 4. Confirmar | El sistema devuelve el ID `INC-0001`, mensaje `"Incidente INC-0001 registrado correctamente."`; el incidente queda con estado `PENDIENTE`, vehículo `"Sin asignar"` y fecha/hora de generación igual al instante del registro; el indicador "incidentes activos" pasa de 0 a 1 y el de "incendios activos" de 0 a 1 |
| CP1.1-02 | **Válido (valor límite).** Verificar que se acepta una ubicación en el borde superior izquierdo de la cuadrícula | Escenario base; existe `INC-0001` | tipo = `ROBO`; fila = 0; columna = 0; gravedad = `BAJA`; descripción = `"Hurto a transeúnte"` | 1. Registrar incidente con fila 0 y columna 0. 2. Confirmar. 3. Consultar el incidente creado | El sistema registra el incidente con ID `INC-0002` y ubicación `(0, 0)`; el indicador "incidentes activos" pasa a 2 y el de "robos activos" a 1 |
| CP1.1-03 | **Inválido (valor límite).** Verificar que se rechaza una ubicación fuera de la cuadrícula | Escenario base con 2 incidentes registrados | tipo = `ACCIDENTE`; fila = 20; columna = 5; gravedad = `MEDIA`; descripción = `"Choque simple"` | 1. Registrar incidente con fila 20. 2. Confirmar | El sistema lanza `InvalidLocationException` y muestra `"La ubicación (20,5) está fuera del mapa."`; no se crea ningún incidente, el contador de incidentes activos sigue en 2 y la aplicación continúa ejecutándose |
| CP1.1-04 | **Inválido.** Verificar que se rechaza el registro sobre una celda de tipo obstáculo y con descripción vacía | Escenario base; la celda (4,4) es `OBSTACULO` | tipo = `ROBO`; fila = 4; columna = 4; gravedad = `ALTA`; descripción = `""` | 1. Registrar incidente en la celda (4,4) con descripción vacía. 2. Confirmar | El sistema lanza `InvalidLocationException` con el mensaje `"La celda (4,4) es un obstáculo y no admite incidentes."`; no se crea el incidente, ninguna estructura se modifica y los indicadores permanecen iguales |

### CP de RF1.2

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF1.2 – Consultar incidente por identificador |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP1.2-01 | **Válido.** Verificar que la consulta por ID existente devuelve la ficha completa del incidente | Existe `INC-0001` (`INCENDIO`, `(12,5)`, `ALTA`, `PENDIENTE`, sin vehículo) | idIncidente = `"INC-0001"` | 1. Abrir el Panel de Incidentes. 2. Digitar `INC-0001` en "Buscar por ID". 3. Ejecutar la búsqueda | El sistema muestra: ID `INC-0001`, tipo `INCENDIO`, ubicación `(12, 5)`, gravedad `ALTA`, fecha/hora en formato `dd-MM-yyyy HH:mm:ss`, descripción, estado `PENDIENTE`, vehículo `"Sin asignar"` y fecha de resolución `"-"`. Ninguna estructura ni indicador cambia |
| CP1.2-02 | **Inválido.** Verificar que la consulta por un ID inexistente informa el error sin cerrar la aplicación | Existen `INC-0001` e `INC-0002`; no existe `INC-9999` | idIncidente = `"INC-9999"` | 1. Digitar `INC-9999` en "Buscar por ID". 2. Ejecutar la búsqueda | El sistema lanza `IncidentNotFoundException` y muestra `"No existe un incidente con el identificador INC-9999."`; no se muestra ficha, la aplicación sigue activa y el estado del sistema no cambia |

### CP de RF1.3

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF1.3 – Listar incidentes activos |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP1.3-01 | **Válido.** Verificar que el listado filtrado por tipo devuelve únicamente los incidentes activos de ese tipo | Existen `INC-0001` (`INCENDIO`, `PENDIENTE`), `INC-0002` (`ROBO`, `PENDIENTE`) e `INC-0003` (`ROBO`, `EN_PROCESO`) | filtroTipo = `ROBO`; filtroEstado = `null` | 1. Abrir el Panel de Incidentes. 2. Seleccionar el filtro de tipo `ROBO`. 3. Aplicar el filtro | La lista muestra exactamente 2 filas: `INC-0002` y `INC-0003`, cada una con ID, tipo, ubicación, gravedad, estado y vehículo; `totalIncidentesActivos` = 2; `INC-0001` no aparece y ninguna estructura se modifica |
| CP1.3-02 | **Válido (clase vacía).** Verificar que el sistema informa correctamente cuando ningún incidente cumple el filtro | Todos los incidentes registrados están en estado `RESUELTO` | filtroTipo = `null`; filtroEstado = `null` | 1. Abrir el Panel de Incidentes. 2. Consultar la lista de incidentes activos sin filtros | El sistema devuelve una lista vacía, `totalIncidentesActivos` = 0 y muestra el mensaje `"No hay incidentes activos."`; **no** se lanza ninguna excepción y la aplicación continúa |

### CP de RF1.4

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF1.4 – Actualizar el estado de un incidente |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP1.4-01 | **Válido (con bonificación).** Verificar que resolver un incidente `ALTA` dentro del tiempo máximo otorga 120 puntos y libera el vehículo | `INC-0001` (`ALTA`) está `EN_PROCESO` con `BOM-001` asignado; han transcurrido 45 s de simulación desde su generación (< 60 s); puntaje del operador = 0 | idIncidente = `"INC-0001"`; nuevoEstado = `RESUELTO` | 1. Seleccionar `INC-0001` en el Panel de Incidentes. 2. Pulsar "Resolver incidente". 3. Confirmar | El incidente queda `RESUELTO` con fecha/hora de resolución registrada; `puntajeObtenido` = 120 (100 base + 20 bonificación) y `puntajeTotalOperador` = 120; `BOM-001` queda `DISPONIBLE` sin incidente asignado; el incidente desaparece del árbol, de la cola de prioridad y del listado de activos, pero sigue consultable por ID; los indicadores "incidentes activos" y "vehículos disponibles" se actualizan |
| CP1.4-02 | **Válido (sin bonificación).** Verificar que resolver un incidente `MEDIA` después del tiempo máximo otorga solo el puntaje base | `INC-0004` (`MEDIA`) está `EN_PROCESO` con `AMB-001`; han transcurrido 150 s de simulación (> 120 s); puntaje del operador = 120 | idIncidente = `"INC-0004"`; nuevoEstado = `RESUELTO` | 1. Seleccionar `INC-0004`. 2. Pulsar "Resolver incidente". 3. Confirmar | `puntajeObtenido` = 70 (sin bonificación) y `puntajeTotalOperador` = 190; el mensaje indica que no se otorgó bonificación por exceder el tiempo máximo; `AMB-001` queda `DISPONIBLE` |
| CP1.4-03 | **Inválido (transición de estados).** Verificar que no se permite resolver un incidente que está `PENDIENTE` y sin vehículo asignado | `INC-0002` está `PENDIENTE` y sin vehículo asignado | idIncidente = `"INC-0002"`; nuevoEstado = `RESUELTO` | 1. Seleccionar `INC-0002`. 2. Pulsar "Resolver incidente". 3. Confirmar | El sistema lanza `InvalidStatusTransitionException` con el mensaje `"No se puede resolver un incidente PENDIENTE sin atención previa."`; `INC-0002` sigue `PENDIENTE`, el puntaje del operador no cambia y ninguna estructura se modifica |

---

## RF2 — Gestionar la prioridad de los incidentes

### CP de RF2.1

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF2.1 – Consultar el incidente de mayor prioridad |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP2.1-01 | **Válido (orden por gravedad).** Verificar que el sistema devuelve el incidente de gravedad `ALTA` aunque no sea el más reciente ni el primero registrado | Activos: `INC-0001` (`BAJA`, 14:00:00), `INC-0002` (`MEDIA`, 14:01:00), `INC-0003` (`ALTA`, 14:02:00) | — (la operación no recibe parámetros) | 1. Abrir el Panel de Incidentes. 2. Consultar "Incidente de mayor prioridad" | El sistema muestra `INC-0003` con gravedad `ALTA`, su tipo, ubicación y fecha/hora; el incidente **no** se retira de la cola de prioridad ni del árbol: al repetir la consulta se obtiene el mismo resultado y el total de activos sigue en 3 |
| CP2.1-02 | **Válido (desempate por antigüedad).** Verificar que ante dos incidentes de igual gravedad se devuelve el más antiguo | Activos: `INC-0005` (`ALTA`, 14:05:00) e `INC-0006` (`ALTA`, 14:03:00) | — | 1. Consultar "Incidente de mayor prioridad" | El sistema muestra `INC-0006` (14:03:00) por ser el más antiguo entre los de gravedad `ALTA`, aplicando el criterio de desempate R6; el resultado coincide con el máximo del árbol binario de búsqueda |
| CP2.1-03 | **Inválido (estructura vacía).** Verificar que la consulta sobre una cola de prioridad vacía se maneja sin cerrar la aplicación | No hay incidentes activos (todos `RESUELTO` o ninguno registrado) | — | 1. Consultar "Incidente de mayor prioridad" | La operación de la estructura lanza `EmptyStructureException`, la aplicación la captura y muestra `"No hay incidentes activos."`; la ventana sigue operativa y el estado del sistema no cambia |

### CP de RF2.2

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF2.2 – Atender el incidente de mayor prioridad |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP2.2-01 | **Válido.** Verificar que "Atender siguiente" extrae el incidente más prioritario y sugiere un vehículo compatible | Activos: `INC-0003` (`ACCIDENTE`, `ALTA`), `INC-0002` (`ROBO`, `MEDIA`); `AMB-001` `DISPONIBLE` | — | 1. Pulsar "Atender siguiente" en el Panel de Incidentes | El sistema pone `INC-0003` como incidente en atención y sugiere `AMB-001`; `INC-0003` se retira de la cola de prioridad, del árbol y de la cola de despacho, pero sigue consultable por ID (RF1.2); el total de pendientes de asignación disminuye en 1; una nueva consulta de mayor prioridad devuelve ahora `INC-0002` |
| CP2.2-02 | **Inválido.** Verificar el comportamiento al pulsar "Atender siguiente" sin incidentes activos | No hay incidentes activos | — | 1. Pulsar "Atender siguiente" | Se lanza `EmptyStructureException`, capturada por la interfaz, que muestra `"No hay incidentes activos."`; no se selecciona ningún incidente ni se sugiere vehículo alguno; las estructuras permanecen vacías y la aplicación continúa |

### CP de RF2.3

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF2.3 – Listar incidentes activos ordenados por prioridad |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP2.3-01 | **Válido.** Verificar que el listado ordena por gravedad descendente y, dentro de cada gravedad, por antigüedad ascendente | Activos: `INC-0001` (`BAJA`, 14:00), `INC-0002` (`MEDIA`, 14:01), `INC-0003` (`ALTA`, 14:02), `INC-0006` (`ALTA`, 14:03) | — | 1. Abrir el Panel de Incidentes. 2. Seleccionar la vista "Ordenados por prioridad" | La lista devuelve exactamente el orden `INC-0003`, `INC-0006`, `INC-0002`, `INC-0001`; el primer elemento coincide con el resultado de RF2.1; el árbol y la cola de prioridad no se modifican y el total de activos sigue en 4 |
| CP2.3-02 | **Válido (clase vacía).** Verificar el listado ordenado cuando no hay incidentes activos | No hay incidentes activos | — | 1. Seleccionar la vista "Ordenados por prioridad" | El sistema devuelve una lista vacía con `totalIncidentesActivos` = 0 y el mensaje `"No hay incidentes activos."`, sin lanzar excepción |

---

## RF3 — Gestionar vehículos de atención

### CP de RF3.1

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF3.1 – Registrar vehículo de atención |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP3.1-01 | **Válido.** Verificar que un vehículo nuevo se registra `DISPONIBLE` y actualiza el indicador de disponibles | Escenario base: 4 vehículos disponibles (`PAT-001`, `PAT-002`, `AMB-001`, `BOM-001`) | idVehiculo = `"AMB-002"`; tipo = `AMBULANCIA`; fila = 8; columna = 14 | 1. Abrir la gestión de vehículos. 2. Seleccionar "Registrar vehículo". 3. Diligenciar ID, tipo y ubicación. 4. Confirmar | El sistema registra `AMB-002` con estado `DISPONIBLE`, sin incidente asignado y ubicación `(8, 14)`; muestra `"Vehículo AMB-002 registrado en (8, 14)."`; el indicador "vehículos disponibles" pasa de 4 a 5 y `AMB-002` es recuperable por RF3.2 |
| CP3.1-02 | **Inválido.** Verificar que se rechaza el registro de un vehículo con identificador ya existente | Escenario base: `PAT-001` ya está registrado | idVehiculo = `"PAT-001"`; tipo = `PATRULLA`; fila = 10; columna = 10 | 1. Seleccionar "Registrar vehículo". 2. Diligenciar el ID `PAT-001`. 3. Confirmar | El sistema lanza `DuplicatedIdException` con el mensaje `"Ya existe un vehículo con el identificador PAT-001."`; no se crea un segundo vehículo, la ubicación de `PAT-001` sigue en `(2, 3)` y el indicador de disponibles no cambia |

### CP de RF3.2

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF3.2 – Consultar vehículo por identificador |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP3.2-01 | **Válido.** Verificar que la consulta devuelve la ficha completa del vehículo, incluido el incidente asignado | `AMB-001` está `EN_RUTA` atendiendo `INC-0003` | idVehiculo = `"AMB-001"` | 1. Abrir la gestión de vehículos. 2. Digitar `AMB-001` en "Buscar por ID". 3. Ejecutar la búsqueda | El sistema muestra: ID `AMB-001`, tipo `AMBULANCIA`, ubicación `(8, 14)`, estado `EN_RUTA` e incidente asignado `INC-0003`; ninguna estructura ni indicador cambia |
| CP3.2-02 | **Inválido.** Verificar que la consulta de un vehículo inexistente informa el error sin cerrar la aplicación | No existe el vehículo `AMB-099` | idVehiculo = `"AMB-099"` | 1. Digitar `AMB-099` en "Buscar por ID". 2. Ejecutar la búsqueda | El sistema lanza `VehicleNotFoundException` y muestra `"No existe un vehículo con el identificador AMB-099."`; no se muestra ficha y la aplicación sigue operativa |

### CP de RF3.3

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF3.3 – Listar vehículos por estado |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP3.3-01 | **Válido.** Verificar que el filtro por estado `DISPONIBLE` coincide con el indicador del Centro de Monitoreo | Escenario base: `PAT-001`, `PAT-002`, `AMB-001` y `BOM-001` `DISPONIBLE`; `BOM-002` `FUERA_DE_SERVICIO` | filtroEstado = `DISPONIBLE`; filtroTipo = `null` | 1. Abrir la gestión de vehículos. 2. Aplicar el filtro de estado `DISPONIBLE` | La lista muestra exactamente 4 filas (`PAT-001`, `PAT-002`, `AMB-001`, `BOM-001`) con ID, tipo, ubicación, estado e incidente; `totalVehiculosFiltrados` = 4, igual al indicador "vehículos disponibles" del Centro de Monitoreo; `BOM-002` no aparece |
| CP3.3-02 | **Válido (clase vacía).** Verificar el comportamiento cuando ningún vehículo cumple la combinación de filtros | Escenario base; ninguna ambulancia está `FUERA_DE_SERVICIO` | filtroEstado = `FUERA_DE_SERVICIO`; filtroTipo = `AMBULANCIA` | 1. Aplicar los filtros estado `FUERA_DE_SERVICIO` y tipo `AMBULANCIA` | El sistema devuelve una lista vacía con `totalVehiculosFiltrados` = 0 y el mensaje `"No hay vehículos que cumplan el filtro."`, sin lanzar excepción |

### CP de RF3.4

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF3.4 – Actualizar el estado de un vehículo |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP3.4-01 | **Válido (transición de estados).** Verificar que un vehículo `EN_RUTA` puede pasar a `ATENDIENDO` al llegar al incidente | `PAT-003` está `EN_RUTA` asignado a `INC-0002`; vehículos disponibles = 3 | idVehiculo = `"PAT-003"`; nuevoEstado = `ATENDIENDO` | 1. Seleccionar `PAT-003` en la gestión de vehículos. 2. Cambiar el estado a `ATENDIENDO`. 3. Confirmar | El vehículo queda `ATENDIENDO` y se muestra `"Vehículo PAT-003 pasó de EN_RUTA a ATENDIENDO."`; el indicador "vehículos disponibles" permanece en 3, porque el vehículo ya no estaba disponible antes del cambio |
| CP3.4-02 | **Inválido (transición de estados).** Verificar que se rechaza poner `FUERA_DE_SERVICIO` un vehículo que tiene un incidente asignado | `AMB-001` está `ATENDIENDO` el incidente `INC-0003` | idVehiculo = `"AMB-001"`; nuevoEstado = `FUERA_DE_SERVICIO` | 1. Seleccionar `AMB-001`. 2. Cambiar el estado a `FUERA_DE_SERVICIO`. 3. Confirmar | El sistema lanza `InvalidStatusTransitionException` con el mensaje `"AMB-001 tiene asignado INC-0003: libere el vehículo antes de retirarlo de servicio."`; `AMB-001` sigue `ATENDIENDO` con su incidente y los indicadores no cambian |

---

## RF4 — Asignar vehículos a incidentes

### CP de RF4.1

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF4.1 – Proponer vehículo candidato |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP4.1-01 | **Válido.** Verificar que el sistema propone el vehículo más específico entre los compatibles y disponibles, sin asignarlo | `INC-0003` es `ACCIDENTE`, `PENDIENTE`; `AMB-001` y `PAT-001` están `DISPONIBLE` | idIncidente = `"INC-0003"` | 1. Seleccionar `INC-0003` en el Panel de Incidentes. 2. Pulsar "Sugerir vehículo" | El sistema propone `AMB-001` (ambulancia antes que patrulla para un accidente) y muestra `"Vehículo sugerido para INC-0003: AMB-001 en (8, 14)."`; **no** se realiza ninguna asignación: `INC-0003` sigue `PENDIENTE` sin vehículo y `AMB-001` sigue `DISPONIBLE` sin incidente |
| CP4.1-02 | **Inválido.** Verificar el comportamiento cuando no existe ningún vehículo disponible y compatible | `INC-0007` es `INCENDIO`, `PENDIENTE`; `BOM-001` está `ATENDIENDO` y `BOM-002` está `FUERA_DE_SERVICIO`; solo quedan patrullas y ambulancias disponibles | idIncidente = `"INC-0007"` | 1. Seleccionar `INC-0007`. 2. Pulsar "Sugerir vehículo" | El sistema lanza `NoCompatibleVehicleException` y muestra `"No hay vehículos disponibles compatibles con INCENDIO."`; no se sugiere ni se asigna vehículo alguno y el estado del sistema no cambia |

### CP de RF4.2

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF4.2 – Asignar vehículo a un incidente |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP4.2-01 | **Válido.** Verificar que una asignación compatible y disponible enlaza incidente y vehículo y actualiza ambos estados | `INC-0007` es `INCENDIO`, `PENDIENTE`, sin vehículo; `BOM-001` está `DISPONIBLE`; vehículos disponibles = 4 | idIncidente = `"INC-0007"`; idVehiculo = `"BOM-001"` | 1. Seleccionar `INC-0007`. 2. Elegir `BOM-001` en la lista de vehículos. 3. Pulsar "Asignar vehículo". 4. Confirmar | El sistema muestra `"BOM-001 asignada a INC-0007. Incidente EN_PROCESO."`; `INC-0007` queda `EN_PROCESO` con vehículo `BOM-001`; `BOM-001` queda `EN_RUTA` con incidente `INC-0007`; el incidente sale de la cola de despacho; "vehículos disponibles" pasa de 4 a 3 y "última acción" devuelve `ASSIGN INC-0007 / BOM-001` |
| CP4.2-02 | **Inválido (tabla de decisión — incompatible).** Verificar que una patrulla no puede ser asignada a un incendio | `INC-0007` es `INCENDIO`, `PENDIENTE`; `PAT-001` está `DISPONIBLE` | idIncidente = `"INC-0007"`; idVehiculo = `"PAT-001"` | 1. Seleccionar `INC-0007`. 2. Elegir `PAT-001`. 3. Pulsar "Asignar vehículo". 4. Confirmar | El sistema lanza `IncompatibleVehicleException` con el mensaje `"Una PATRULLA no puede atender un INCENDIO."`; `INC-0007` sigue `PENDIENTE` sin vehículo, `PAT-001` sigue `DISPONIBLE` sin incidente, el indicador de disponibles no cambia y la pila de acciones no registra nada |
| CP4.2-03 | **Inválido (no disponible).** Verificar que un vehículo compatible pero ocupado no puede ser asignado | `INC-0008` es `INCENDIO`, `PENDIENTE`; `BOM-002` es compatible pero está `FUERA_DE_SERVICIO` | idIncidente = `"INC-0008"`; idVehiculo = `"BOM-002"` | 1. Seleccionar `INC-0008`. 2. Elegir `BOM-002`. 3. Pulsar "Asignar vehículo". 4. Confirmar | El sistema lanza `VehicleNotAvailableException` con el mensaje `"El vehículo BOM-002 no está disponible (FUERA_DE_SERVICIO)."`; ni el incidente ni el vehículo cambian de estado |
| CP4.2-04 | **Inválido (incidente resuelto).** Verificar que un incidente `RESUELTO` no puede recibir vehículos | `INC-0001` está `RESUELTO`; `PAT-002` está `DISPONIBLE` | idIncidente = `"INC-0001"`; idVehiculo = `"PAT-002"` | 1. Buscar `INC-0001` por ID. 2. Elegir `PAT-002`. 3. Pulsar "Asignar vehículo" | El sistema lanza `InvalidAssignmentException` con el mensaje `"El incidente INC-0001 ya está resuelto y no admite asignaciones."`; `PAT-002` sigue `DISPONIBLE` y el puntaje del operador no cambia |
| CP4.2-05 | **Inválido (identificador inexistente).** Verificar que se valida primero la existencia del incidente | No existe `INC-9999`; `AMB-001` está `DISPONIBLE` | idIncidente = `"INC-9999"`; idVehiculo = `"AMB-001"` | 1. Digitar `INC-9999` como incidente. 2. Elegir `AMB-001`. 3. Pulsar "Asignar vehículo" | El sistema lanza `IncidentNotFoundException` con el mensaje `"No existe un incidente con el identificador INC-9999."` antes de validar el vehículo; `AMB-001` permanece `DISPONIBLE` y la aplicación sigue operativa |

### CP de RF4.3

| | |
|---|---|
| **ID - Nombre Requerimiento / Historia de usuario** | RF4.3 – Liberar vehículo al finalizar la atención |
| **Sprint / Iteración** | Entrega 1 |

| ID Caso | Nombre Caso (Objetivo) | Precondición | Datos de Entrada | Pasos | Resultado Esperado |
|---|---|---|---|---|---|
| CP4.3-01 | **Válido (cancelación).** Verificar que al cancelar una asignación el vehículo queda disponible y el incidente vuelve a la cola de despacho | `INC-0003` está `EN_PROCESO` con `AMB-001` `EN_RUTA`; vehículos disponibles = 3 | idIncidente = `"INC-0003"`; esCancelacion = `true` | 1. Seleccionar `INC-0003`. 2. Pulsar "Cancelar asignación". 3. Confirmar | `AMB-001` queda `DISPONIBLE`, sin incidente asignado y ubicada en `(12, 5)` (celda del incidente); `INC-0003` vuelve a `PENDIENTE` sin vehículo y se reinserta al final de la cola de despacho; "vehículos disponibles" pasa de 3 a 4; el mensaje muestra `"AMB-001 liberada y disponible en (12, 5)."` |
| CP4.3-02 | **Inválido.** Verificar que no se puede liberar un vehículo de un incidente que no tiene asignación | `INC-0002` está `PENDIENTE` y sin vehículo asignado | idIncidente = `"INC-0002"`; esCancelacion = `true` | 1. Seleccionar `INC-0002`. 2. Pulsar "Cancelar asignación" | El sistema lanza `InvalidAssignmentException` con el mensaje `"El incidente INC-0002 no tiene ningún vehículo asignado."`; el incidente sigue `PENDIENTE`, ningún vehículo cambia de estado y el indicador de disponibles no varía |

---

## Resumen de cobertura

| Requerimiento | Casos válidos | Casos inválidos | Total |
|---|---|---|---|
| RF1.1 – Registrar incidente | CP1.1-01, CP1.1-02 | CP1.1-03, CP1.1-04 | 4 |
| RF1.2 – Consultar incidente | CP1.2-01 | CP1.2-02 | 2 |
| RF1.3 – Listar activos | CP1.3-01, CP1.3-02 | — (clase vacía cubierta en CP1.3-02) | 2 |
| RF1.4 – Actualizar estado | CP1.4-01, CP1.4-02 | CP1.4-03 | 3 |
| RF2.1 – Consultar prioritario | CP2.1-01, CP2.1-02 | CP2.1-03 | 3 |
| RF2.2 – Atender prioritario | CP2.2-01 | CP2.2-02 | 2 |
| RF2.3 – Listar por prioridad | CP2.3-01, CP2.3-02 | — (clase vacía cubierta en CP2.3-02) | 2 |
| RF3.1 – Registrar vehículo | CP3.1-01 | CP3.1-02 | 2 |
| RF3.2 – Consultar vehículo | CP3.2-01 | CP3.2-02 | 2 |
| RF3.3 – Listar vehículos | CP3.3-01, CP3.3-02 | — (clase vacía cubierta en CP3.3-02) | 2 |
| RF3.4 – Actualizar estado vehículo | CP3.4-01 | CP3.4-02 | 2 |
| RF4.1 – Proponer candidato | CP4.1-01 | CP4.1-02 | 2 |
| RF4.2 – Asignar vehículo | CP4.2-01 | CP4.2-02 … CP4.2-05 | 5 |
| RF4.3 – Liberar vehículo | CP4.3-01 | CP4.3-02 | 2 |
| **Total** | **20** | **15** | **35** |

**Completitud** = casos de prueba / total funcionalidades = 35 / 14 = **2.50** (indicador para el README, primera iteración).
