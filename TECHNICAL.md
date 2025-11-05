# Documentación Técnica - Sistema de Gestión de Talleres Mecánicos

## 📚 Tabla de Contenidos

1. [Arquitectura General](#arquitectura-general)
2. [Módulos Principales](#módulos-principales)
3. [Manejo de Datos](#manejo-de-datos)
4. [Flujos de Operación](#flujos-de-operación)
5. [Patrones de Diseño](#patrones-de-diseño)
6. [Consideraciones de Rendimiento](#consideraciones-de-rendimiento)

## Arquitectura General

### Diagrama de Relaciones

```
Cliente
├── Vehiculos[] (N vehículos por cliente)
    ├── IncidenciasDetectadas[] (N incidencias por vehículo)
    │   ├── MecanicosAsignados[] (N mecánicos por incidencia)
    └── Estado (En taller, Disponible, etc.)

Mecanico
├── Especialidad (mecánica, eléctrica, carrocería)
├── Activo (disponible o de baja)
└── PlazasTaller[] (N plazas por mecánico)

Plaza Taller
├── Mecanico (asociado)
├── Vehiculo (actual)
└── Estado (Ocupada, libre)
```

## Módulos Principales

### 1. Gestión de Clientes

**Responsabilidades:**
- Crear nuevos clientes con información de contacto
- Modificar datos de cliente existentes
- Eliminar clientes y sus datos asociados
- Listar todos los clientes

**Funciones Clave:**
- `CrearCliente()`: Solicita entrada de usuario y añade cliente
- `VerClientes()`: Lista clientes con ID y nombre
- `ModificarCliente()`: Permite editar o eliminar cliente
- `ListarClientesEnTaller()`: Muestra clientes con vehículos en reparación

**Cascada de Eliminación:**
```
Eliminar Cliente
├── Eliminar Vehiculos del cliente
│   ├── Eliminar Incidencias asociadas
│   │   └── Desasignar Mecanicos
│   └── Liberar Plazas de taller
```

### 2. Gestión de Vehículos

**Responsabilidades:**
- Crear vehículos asociados a clientes
- Rastrear estado del vehículo (Disponible, En taller)
- Modificar información del vehículo
- Eliminar vehículos

**Funciones Clave:**
- `CrearVehiculo()`: Método de Cliente para añadir vehículo
- `ModificarVehiculo()`: Edita o elimina vehículo
- `ListarVehiculosCliente()`: Muestra vehículos de un cliente
- `AsignarVehiculoTaller()`: Coloca vehículo en plaza de taller

**Atributos Importantes:**
- `FechaEntrada`: Timestamp de ingreso al taller
- `FechaEstimSalida`: Fecha estimada de salida
- `IncidenciasDetectadas`: Problemas asociados
- `Estado`: Disponible o En taller

### 3. Gestión de Incidencias

**Responsabilidades:**
- Registrar problemas detectados en vehículos
- Asignar incidencias a mecánicos específicos
- Rastrear el estado de resolución
- Modificar o cerrar incidencias

**Funciones Clave:**
- `CrearIncidencia()`: Registra nuevo problema
- `CambiarEstadoIncidencia()`: Actualiza estado (abierta → en proceso → cerrada)
- `ModificarIncidencia()`: Edita detalles
- `ListarIncidenciasVehiculo()`: Muestra problemas de un vehículo
- `ListarIncidenciasMecanico()`: Muestra incidencias asignadas a mecánico

**Tipos de Incidencia:**
- Mecánica (cambios, reparaciones mecánicas)
- Eléctrica (sistemas eléctricos, batería)
- Carrocería (abolladuras, pintura, herrumbre)

**Niveles de Prioridad:**
- Baja: Revisiones, mantenimiento preventivo
- Media: Problemas que afectan comodidad
- Alta: Problemas que afectan seguridad

### 4. Gestión de Mecánicos

**Responsabilidades:**
- Registrar personal del taller
- Asignar especialidades
- Gestionar disponibilidad (alta/baja)
- Asignar plazas de trabajo

**Funciones Clave:**
- `CrearMecanico()`: Registra nuevo mecánico y asigna plazas
- `VerMecanicos()`: Lista mecánicos con detalles
- `ModificarMecanico()`: Edita información del mecánico
- `ListarMecanicosDisponibles()`: Muestra mecánicos sin incidencias asignadas
- `DarAltaBajaMecanico()`: Cambia estado activo/inactivo

**Especialidades:**
- Mecánica (motor, transmisión, suspensión)
- Eléctrica (sistema eléctrico, electrónica)
- Carrocería (estructura, pintura, accesorios)

**Gestión de Plazas:**
- Cada mecánico recibe 2 plazas automáticamente
- Las plazas se crean al registrar mecánico
- Se liberan al eliminar mecánico

### 5. Gestión de Plazas de Taller

**Responsabilidades:**
- Gestionar espacios disponibles en el taller
- Asignar vehículos a plazas específicas
- Liberar plazas cuando se completa trabajo

**Lógica de Asignación:**
```go
AsignarVehiculoTaller() {
    1. Buscar vehículo por ID
    2. Buscar plaza disponible (estado == "")
    3. Asignar vehículo a plaza
    4. Cambiar estado plaza a "Ocupada"
    5. Cambiar estado vehículo a "En taller"
}
```

## Manejo de Datos

### Uso de Punteros

El sistema usa punteros extensivamente:

```go
// En structs
type Cliente struct {
    Vehiculos []*Vehiculo  // Slice de punteros
}

// En asignaciones
ptr := &vehiculo  // Dirección del struct
c.Vehiculos = append(c.Vehiculos, ptr)  // Almacenar puntero
```

**Ventajas:**
- Evita copias de structs grandes
- Cambios reflejados en todas las referencias
- Memoria más eficiente

### Variables Globales

```go
var clientes []*Cliente      // Slice dinámico de clientes
var vehiculos []*Vehiculo    // Slice dinámico de vehículos
var incidencias []*Incidencia // Slice dinámico de incidencias
var mecanicos []*Mecanico    // Slice dinámico de mecánicos
var plazasTaller []*plaza    // Slice dinámico de plazas
```

**Limitaciones:**
- No hay persistencia entre sesiones
- Datos residen en memoria
- No hay sincronización para acceso concurrente

## Flujos de Operación

### Flujo 1: Cliente Registra Vehículo

```
1. Crear Cliente (ID, Nombre, Teléfono, Email)
   └─> Añadido a slice global clientes[]

2. Crear Vehículo para Cliente
   └─> Cliente.CrearVehiculo()
   │   ├─> Solicita datos (Matrícula, Marca, Modelo)
   │   ├─> Crea puntero a Vehículo
   │   ├─> Añade a Cliente.Vehiculos[]
   │   └─> Añade a vehiculos[] global

3. Resultado: Vehículo disponible y asociado al cliente
```

### Flujo 2: Detectar Problema e Iniciar Reparación

```
1. Crear Incidencia
   ├─> Solicita: ID, Descripción, Tipo, Prioridad
   ├─> Estado = "abierta"
   ├─> Añade a incidencias[] global
   ├─> Asocia a Vehículo.IncidenciasDetectadas[]
   └─> Asigna a Mecánico.MecanicosAsignados[]

2. Asignar Vehículo a Plaza de Taller
   ├─> Busca plaza disponible (estado == "")
   ├─> Asigna vehiculo a plaza.vehiculo
   ├─> Cambia plaza.estado = "Ocupada"
   └─> Cambia vehiculo.Estado = "En taller"

3. Resultado: Vehículo en taller, incidencia asignada a mecánico
```

### Flujo 3: Completar Reparación

```
1. Cambiar Estado Incidencia
   ├─> abierta → en proceso → cerrada
   └─> Busca incidencia y actualiza

2. Liberar Plaza de Taller
   ├─> Busca plaza con vehiculo
   ├─> plaza.vehiculo = nil
   ├─> plaza.estado = ""
   └─> Vehiculo.Estado = "Disponible"

3. Resultado: Plaza disponible, vehículo listo para entrega
```

### Flujo 4: Eliminar Cliente

```
Eliminar Cliente
├─> Buscar cliente por ID
├─> Para cada Vehículo del cliente:
│   ├─> Para cada Incidencia del vehículo:
│   │   └─> Desasignar Mecánicos
│   ├─> Liberar Plazas ocupadas por vehículo
│   └─> Eliminar de vehiculos[] global
└─> Eliminar de clientes[] global
```

## Patrones de Diseño

### 1. Búsqueda e Iteración

**Patrón común:**
```go
func ListarIncidenciasVehiculo() {
    var id int
    fmt.Scan(&id)
    for _, v := range vehiculos {
        if v.ID == id {
            // Procesar vehículo encontrado
            return
        }
    }
    fmt.Println("No encontrado")
}
```

**Mejora recomendada:** Usar índices para eliminaciones:
```go
for i := 0; i < len(vehiculos); i++ {
    if vehiculos[i].ID == id {
        // Poder eliminar: vehiculos = append(vehiculos[:i], vehiculos[i+1:]...)
        break
    }
}
```

### 2. Cascada de Operaciones

**Al eliminar entidad:**
- Actualizar referencias en otras entidades
- Liberar recursos asociados
- Mantener consistencia referencial

**Ejemplo en ModificarCliente():**
```go
// Al eliminar cliente:
// 1. Eliminar vehículos del cliente
// 2. Eliminar incidencias de los vehículos
// 3. Liberar plazas del taller
// 4. Finalmente eliminar cliente
```

### 3. Gestión de Estados

**Estados Vehículo:**
- Disponible: Listo para servicio
- En taller: Actualmente siendo reparado

**Estados Incidencia:**
- abierta: Registrada, sin iniciar
- en proceso: Mecánico trabajando
- cerrada: Trabajo completado

**Estados Mecánico:**
- Activo (true): Disponible para trabajo
- Baja (false): No disponible

## Consideraciones de Rendimiento

### Complejidad de Operaciones

| Operación | Complejidad | Nota |
|-----------|------------|------|
| Buscar cliente por ID | O(n) | Búsqueda lineal |
| Crear vehículo | O(1) | Append al slice |
| Eliminar vehículo | O(n) | Requiere reorganizar |
| Listar incidencias | O(n×m) | Cliente × Incidencias |
| Asignar plaza | O(n) | Busca de plaza disponible |

### Limitaciones Actuales

1. **Búsquedas lineales**: Para n grandes, considerar índices (hash map)
2. **Sin validación de entrada**: Añadir checks de validez
3. **Sin sincronización**: Problemas con acceso concurrente
4. **Memoria no liberada**: Datos persisten durante sesión

### Optimizaciones Posibles

```go
// Usar map para búsquedas O(1)
var clientesMap map[int]*Cliente

// Validar entrada
func validarID(id int) bool {
    return id > 0
}

// Usar goroutines para operaciones paralelas
go procesarIncidencia(inc)
```

## Consideraciones de Seguridad

### Validaciones Necesarias

- Verificar IDs válidos (> 0)
- Validar formato de email
- Validar teléfono
- Verificar existencia antes de operar

### Casos de Error No Controlados

```go
// Actual (puede causar panic)
fmt.Scan(&id)  // ¿Qué si entrada no es número?

// Mejorado
n, err := fmt.Scan(&id)
if err != nil {
    fmt.Println("Entrada inválida")
    return
}
```

## Extensiones Futuras

### Base de Datos
```go
// Reemplazar slices globales con BD
type Repository struct {
    db *sql.DB
}

func (r *Repository) GetCliente(id int) (*Cliente, error) {
    // Consultar BD
}
```

### API REST
```go
func main() {
    http.HandleFunc("/api/clientes", handleClientes)
    http.HandleFunc("/api/vehiculos", handleVehiculos)
    http.ListenAndServe(":8080", nil)
}
```

### Concurrencia
```go
// Usar mutex para datos compartidos
var mu sync.RWMutex

func (r *Repository) GetCliente(id int) *Cliente {
    mu.RLock()
    defer mu.RUnlock()
    // Acceso seguro
}
```