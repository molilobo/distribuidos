# Sistema de Gestión de Talleres Mecánicos

Un sistema completo de gestión escrito en **Go** para administrar operaciones de talleres mecánicos, incluyendo gestión de clientes, vehículos, incidencias y mecánicos.

## Características Principales

- **Gestión de Clientes**: Crear, modificar y eliminar clientes con sus datos de contacto
- **Gestión de Vehículos**: Administrar vehículos asociados a clientes con seguimiento de estado
- **Gestión de Incidencias**: Crear y rastrear problemas mecánicos, eléctricos y de carrocería
- **Gestión de Mecánicos**: Administrar personal con especialidades y disponibilidad
- **Sistema de Plazas**: Asignar vehículos a plazas de taller con mecánicos específicos
- **Seguimiento de Estados**: Monitorear el progreso de incidencias y vehículos en taller

### Instalación

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/sistema-taller-mecanico.git
cd sistema-taller-mecanico

# Compilar el programa
go build -o taller main.go
```

### Ejecución

```bash
# Ejecutar el programa
./taller
```

O compilar y ejecutar directamente:

```bash
go run main.go
```

## 📖 Estructura del Menú

El programa utiliza un sistema de menús interactivos:

### Menú Principal
1. **Gestión Taller** - Asignar vehículos a plazas y listar vehículos en taller
2. **Gestión Vehículos** - Crear, visualizar y modificar vehículos
3. **Gestión Clientes** - Administrar información de clientes
4. **Gestión Incidencias** - Crear y gestionar problemas detectados
5. **Gestión Mecánicos** - Administrar personal del taller
6. **Salir** - Cerrar la aplicación

## Estructura de Datos

### Cliente
```go
type Cliente struct {
    ID        int
    Nombre    string
    Telefono  string
    Email     string
    Vehiculos []*Vehiculo
}
```

### Vehículo
```go
type Vehiculo struct {
    ID                    int
    Matricula             string
    Marca                 string
    Modelo                string
    FechaEntrada          time.Time
    FechaEstimSalida      time.Time
    IncidenciasDetectadas []*Incidencia
    Estado                string
}
```

### Incidencia
```go
type Incidencia struct {
    ID                 int
    MecanicosAsignados []*Mecanico
    Tipo               string      // mecánica, eléctrica, carrocería
    Prioridad          string      // baja, media, alta
    Descripcion        string
    Estado             string      // abierta, en proceso, cerrada
}
```

### Mecánico
```go
type Mecanico struct {
    ID               int
    Nombre           string
    Especialidad     string // mecánica, eléctrica, carrocería
    AniosExperiencia int
    Activo           bool
}
```

### Plaza de Taller
```go
type plaza struct {
    estado   string
    mecanico *Mecanico
    vehiculo *Vehiculo
}
```

## Guía de Uso Rápido

### Crear un Cliente
1. Seleccionar opción **3** (Gestión Clientes)
2. Seleccionar opción **1** (Crear cliente)
3. Ingresar ID, nombre, teléfono y email

### Agregar Vehículo a Cliente
1. Seleccionar opción **2** (Gestión Vehículos)
2. Seleccionar opción **1** (Crear vehículo)
3. Ingresar ID del cliente
4. Llenar datos del vehículo (matrícula, marca, modelo)

### Crear Mecánico
1. Seleccionar opción **5** (Gestión Mecánicos)
2. Seleccionar opción **1** (Crear mecánico)
3. Ingresar datos y especialidad

### Asignar Vehículo a Taller
1. Seleccionar opción **1** (Gestión Taller)
2. Seleccionar opción **1** (Asignar vehículo a plaza)
3. Ingresar ID del vehículo

### Crear Incidencia
1. Seleccionar opción **4** (Gestión Incidencias)
2. Seleccionar opción **1** (Crear incidencia)
3. Ingresar detalles (tipo, prioridad, descripción)
4. Asociar a vehículo y mecánico

## Características Avanzadas

### Estados de Incidencia
- **Abierta**: Incidencia registrada, sin comenzar
- **En proceso**: Mecánico trabajando en la reparación
- **Cerrada**: Trabajo completado

### Disponibilidad de Mecánicos
- Consultar mecánicos disponibles (no asignados a incidencias activas)
- Dar de alta/baja mecánicos según necesidad
- Cada mecánico tiene plazas de trabajo asignadas automáticamente

### Gestión de Eliminación
- Eliminar cliente elimina automáticamente sus vehículos
- Eliminar vehículo libera las plazas de taller ocupadas
- Eliminar mecánico libera sus plazas y desasigna incidencias

## Variables Globales

El sistema mantiene cuatro slices globales:
- `clientes`: Lista de todos los clientes
- `vehiculos`: Lista de todos los vehículos
- `incidencias`: Lista de todas las incidencias
- `mecanicos`: Lista de todos los mecánicos
- `plazasTaller`: Plazas disponibles en el taller

##  Función Demo

El código incluye una función `Demo()` que precarga datos de ejemplo:
- 2 mecánicos (Juan Pérez - Mecánica, Ana López - Eléctrica)
- 1 cliente (Carlos García)
- 1 vehículo (Toyota Corolla)
- 1 incidencia de cambio de aceite

Para usar la demo, descomentar `Demo()` en la función `main()`.

## Métodos Principales

### Cliente
- `CrearVehiculo()`: Añade vehículo al cliente
- `VerVehiculos()`: Lista vehículos del cliente

### Funciones Globales
- `AsignarVehiculoTaller()`: Asigna vehículo a una plaza
- `ListarClientesEnTaller()`: Muestra clientes con vehículos en reparación
- `ModificarCliente()`: Edita datos del cliente
- `ModificarVehiculo()`: Edita datos del vehículo
- `ModificarIncidencia()`: Edita datos de incidencia
- `ModificarMecanico()`: Edita datos del mecánico
- `DarAltaBajaMecanico()`: Cambia estado activo/inactivo
## Autor

Raul Molina Looez.
