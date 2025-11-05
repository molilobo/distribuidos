# Ejemplos de Uso - Sistema de Gestión de Talleres Mecánicos

## 📚 Tabla de Contenidos

1. [Ejemplo 1: Flujo Completo de un Cliente](#ejemplo-1-flujo-completo-de-un-cliente)
2. [Ejemplo 2: Gestión de Incidencias](#ejemplo-2-gestión-de-incidencias)
3. [Ejemplo 3: Gestión de Mecánicos](#ejemplo-3-gestión-de-mecánicos)
4. [Ejemplo 4: Casos de Error](#ejemplo-4-casos-de-error)

## Ejemplo 1: Flujo Completo de un Cliente

### Escenario
Un cliente nuevo lleva su vehículo al taller para una revisión y reparación.

### Paso a Paso

#### 1. Registrar Cliente

```
--- Menú Principal ---
1. Gestión Taller
2. Gestión Vehículos
3. Gestión Clientes
4. Gestión Incidencias
5. Gestión Mecánicos
6. Salir
Seleccione opción: 3

1. Crear cliente
2. Ver clientes
3. Modificar Cliente
Seleccione opción: 1

ID cliente: 1
Nombre: Juan García
Teléfono: 555-123456
Email: juan@gmail.com
Cliente creado.
```

**Resultado:** Cliente con ID 1 registrado

#### 2. Registrar Vehículo del Cliente

```
Menú Principal → 2 (Gestión Vehículos)

1. Crear vehículo
2. Ver vehículos de cliente
3. Listar incidencias de vehículo
4. Modificar vehículo
Seleccione opción: 1

ID cliente: 1
Ingrese ID del vehículo: 101
Ingrese matrícula: ABC-1234
Ingrese marca: Toyota
Ingrese modelo: Corolla
Vehículo creado con éxito.
```

**Resultado:** Vehículo Toyota Corolla registrado para Juan García

#### 3. Crear Incidencia (Problema Detectado)

```
Menú Principal → 4 (Gestión Incidencias)

1. Crear incidencia
2. Ver incidencias
3. Cambiar estado de incidencia
4. Modificar incidencia
Seleccione opción: 1

ID incidencia: 1001
Descripción: Revisión general y cambio de aceite
Tipo (mecánica/eléctrica/carrocería): mecánica
Prioridad (baja/media/alta): media
ID del vehículo para asignar la incidencia: 101
ID del mecanico para asignar la incidencia: 1
Incidencia creada.
```

**Resultado:** Incidencia registrada y asignada al mecánico 1

#### 4. Asignar Vehículo a Plaza de Taller

```
Menú Principal → 1 (Gestión Taller)

1. Asignar vehículo a plaza
2. Listar clientes con vehículos en taller
Seleccione opción: 1

Ingrese ID del vehículo a asignar al taller: 101
Vehículo ABC-1234 asignado a plaza con mecánico Juan Pérez
```

**Resultado:** Vehículo en taller, bajo cuidado del mecánico

#### 5. Actualizar Estado de Incidencia

```
Menú Principal → 4 (Gestión Incidencias)

Seleccione opción: 3 (Cambiar estado)

Ingrese ID de la incidencia: 1001
Ingrese nuevo estado (abierta/en proceso/cerrada): en proceso
Estado actualizado.

... (después de terminar el trabajo) ...

Ingrese ID de la incidencia: 1001
Ingrese nuevo estado (abierta/en proceso/cerrada): cerrada
Estado actualizado.
```

**Resultado:** Incidencia completada

#### 6. Verificar Clientes en Taller

```
Menú Principal → 1 (Gestión Taller)

Seleccione opción: 2 (Listar clientes en taller)

Clientes con vehículos en taller:
Cliente: Juan García, Vehículo: ABC-1234 (Corolla)
```

---

## Ejemplo 2: Gestión de Incidencias

### Escenario Múltiple

Un vehículo tiene varios problemas que necesitan atención de diferentes especialistas.

#### Vehículo: Renault Megane (ID: 102, Matricula: XYZ-5678)

##### Incidencia 1: Problema Eléctrico

```
Crear incidencia:
  ID: 2001
  Descripción: Batería descargada, revisar alternador
  Tipo: eléctrica
  Prioridad: alta
  Asignar a: Mecánico 2 (Ana López - Especialidad: Eléctrica)
```

##### Incidencia 2: Problema de Carrocería

```
Crear incidencia:
  ID: 2002
  Descripción: Abolladuras en puerta lateral izquierda
  Tipo: carrocería
  Prioridad: baja
  Asignar a: Mecánico 3 (Carlos Ruiz - Especialidad: Carrocería)
```

### Estados de Progreso

**Día 1:**
```
Incidencia 2001: abierta → en proceso (Ana López empezó)
Incidencia 2002: abierta (en espera)
```

**Día 2:**
```
Incidencia 2001: en proceso → cerrada (Batería nueva instalada)
Incidencia 2002: abierta → en proceso (Carlos empezó)
```

**Día 3:**
```
Incidencia 2001: cerrada ✓
Incidencia 2002: en proceso → cerrada (Puerta reparada y pintada)
```

### Ver Incidencias

```
Menú Principal → 2 (Gestión Vehículos)
Seleccione opción: 3

Ingrese ID del vehículo: 102

Incidencias del vehículo:
ID: 2001, Tipo: eléctrica, Prioridad: alta, Estado: cerrada, Descripción: Batería descargada, revisar alternador
ID: 2002, Tipo: carrocería, Prioridad: baja, Estado: cerrada, Descripción: Abolladuras en puerta lateral izquierda
```

---

## Ejemplo 3: Gestión de Mecánicos

### Registrar Mecánicos con Especialidades Diferentes

#### Crear Mecánicos

```
Menú Principal → 5 (Gestión Mecánicos)
Seleccione opción: 1 (Crear mecánico)

MECÁNICO 1:
ID mecánico: 1
Nombre: Juan Pérez
Especialidad: mecánica
(2 plazas automáticas asignadas)

MECÁNICO 2:
ID mecánico: 2
Nombre: Ana López
Especialidad: eléctrica
(2 plazas automáticas asignadas)

MECÁNICO 3:
ID mecánico: 3
Nombre: Carlos Ruiz
Especialidad: carrocería
(2 plazas automáticas asignadas)

Total: 6 plazas disponibles en el taller
```

#### Ver Disponibilidad

```
Menú Principal → 5 (Gestión Mecánicos)
Seleccione opción: 3 (Listar mecánicos disponibles)

Mecánicos disponibles:
ID: 1, Nombre: Juan Pérez, Especialidad: mecánica
ID: 3, Nombre: Carlos Ruiz, Especialidad: carrocería

(Ana López no aparece porque está asignada a incidencias activas)
```

#### Dar de Baja Mecánico

```
Menú Principal → 5 (Gestión Mecánicos)
Seleccione opción: 6 (Dar Baja/Alta Mecanicos)

Ingrese ID del mecánico: 1
Estado de Juan Pérez actual : true
1. Dar de alta 
2. Dar de baja 
Seleccione opción: 2

(Mecánico Juan Pérez está ahora de baja, sus plazas se liberan)
```

---

## Ejemplo 4: Casos de Error

### Error 1: Cliente no Encontrado

```
Menú Principal → 2 (Gestión Vehículos)
Seleccione opción: 1 (Crear vehículo)

ID cliente: 999
Cliente no encontrado.
```

### Error 2: No hay Plazas Disponibles

```
Menú Principal → 1 (Gestión Taller)
Seleccione opción: 1 (Asignar vehículo)

Ingrese ID del vehículo a asignar al taller: 101

No hay plazas disponibles en el taller.

(Solución: Dar de alta más mecánicos o liberar plazas)
```

### Error 3: Vehículo con Múltiples Incidencias

```
Menú Principal → 2 (Gestión Vehículos)
Seleccione opción: 3 (Listar incidencias)

Ingrese ID del vehículo: 102

Incidencias del vehículo:
ID: 2001, Tipo: eléctrica, Prioridad: alta, Estado: en proceso, Descripción: Batería
ID: 2002, Tipo: carrocería, Prioridad: baja, Estado: abierta, Descripción: Abolladuras
ID: 2003, Tipo: mecánica, Prioridad: media, Estado: en proceso, Descripción: Frenos

(Trabajo coordinado entre 3 mecánicos)
```

---

## 📊 Sesión Completa de Demo

### Usar Función Demo

Descomenta en `main()`:

```go
func main() {
    // ... código ...
    Demo()  // <-- Descomenta esto
    menu()
}
```

Al ejecutar, carga automáticamente:

```
=== Demo cargada ===

--- Menú Principal ---
1. Gestión Taller
2. Gestión Vehículos
3. Gestión Clientes
4. Gestión Incidencias
5. Gestión Mecánicos
6. Salir
```

### Estado Inicial con Demo

```
Clientes: 1
  - Carlos García (ID: 1)
  
Vehículos: 1
  - Toyota Corolla ABC-123 (En taller)
  
Incidencias: 1
  - Cambio de aceite (Alta prioridad, Abierta)
  
Mecánicos: 2
  - Juan Pérez (Mecánica, 5 años, Activo)
  - Ana López (Eléctrica, 3 años, Activa)
  
Plazas: 4
  - 2 para Juan Pérez
  - 2 para Ana López
```

---

## 🔄 Flujos de Casos Reales

### Caso 1: Taller Ocupado

**Mañana - Llegan 3 clientes:**

```
Hora 8:00 AM - Cliente 1
  Vehículo 1 → Asignar a Plaza 1 (Juan Pérez)
  Incidencia: Revisión general (abierta)

Hora 8:30 AM - Cliente 2
  Vehículo 2 → Asignar a Plaza 2 (Juan Pérez)
  Incidencia: Cambio de neumáticos (abierta)

Hora 9:00 AM - Cliente 3
  Vehículo 3 → Asignar a Plaza 3 (Ana López)
  Incidencia: Revisar sistema eléctrico (abierta)

Hora 9:15 AM - Cliente 4
  Vehículo 4 → No hay plazas disponibles
  ESPERA: Cliente debe aguardar o volver más tarde
```

### Caso 2: Finalizar Trabajos

**Tarde - Se liberan plazas:**

```
Hora 12:00 PM
  Vehículo 1 completado:
    Incidencia: abierta → en proceso → cerrada
    Plaza 1 → Liberada
    Cliente 1 llamado para recoger

Hora 1:00 PM
  Vehículo 4 (que esperaba) → Asignado a Plaza 1 (Juan Pérez)

Hora 2:00 PM
  Vehículo 2 completado → Plaza 2 liberada
  Vehículo 3 completado → Plaza 3 liberada
```

---

## ⚙️ Operaciones Avanzadas

### Modificar Vehículo en Taller

```
Menú Principal → 2 (Gestión Vehículos)
Seleccione opción: 4 (Modificar vehículo)

Ingrese el ID del vehiculo: 101
Vehículo encontrado
1. Modificar datos
2. Eliminar
Seleccione opción: 1

Ingrese nueva matrícula: ABC-1235
Ingrese nueva marca: Toyota
Ingrese nuevo modelo: Corolla Hybrid
Vehículo modificado con éxito.
```

### Eliminar Cliente y Todo lo Asociado

```
Menú Principal → 3 (Gestión Clientes)
Seleccione opción: 3 (Modificar Cliente)

Ingrese el ID del cliente: 1
Cliente encontrado: Juan García
1. Modificar datos
2. Eliminar cliente
Seleccione opción: 2

Eliminando:
  - Vehículo ABC-1234 (Toyota Corolla)
    - Incidencia 1001 (Revisión general)
      - Desasignando Mecánico Juan Pérez
    - Liberando Plaza 1
    
Cliente y sus vehículos eliminados con éxito.
```

---

## 📈 Estadísticas por Simulación

### Día Típico de Taller

```
Entrada:
  - 5 clientes nuevos
  - 3 clientes con seguimiento
  - 2 clientes retrasados

Salida:
  - 4 clientes completados
  - 4 clientes en proceso
  - 2 clientes pendientes

Recursos:
  - Plazas usadas: 4/6 (66%)
  - Mecánicos activos: 3/3 (100%)
  - Incidencias abiertas: 2
  - Incidencias en proceso: 4
  - Incidencias cerradas: 8
```

---

## 💡 Consejos de Uso

1. **Registrar Mecánicos Primero**
   - Esto crea las plazas del taller automáticamente

2. **Usar IDs Secuenciales**
   - Clientes: 1, 2, 3...
   - Vehículos: 101, 102, 103...
   - Incidencias: 1001, 1002, 1003...
   - Mecánicos: 1, 2, 3...

3. **Monitorear Plazas Disponibles**
   - Dar de baja mecánicos con carga baja
   - Contratar temporales en temporada alta

4. **Prioridades para Asignación**
   - Alta → Asignar inmediatamente
   - Media → Cola de trabajo
   - Baja → Agrupar con otros trabajos

5. **Usar Demo para Práctica**
   - Experimentar sin datos en blanco
   - Entender flujos rápidamente