# 08 - Grids en PML: TABLE y NetGridControl

## 📋 ¿Qué tipos de grid existen en PML?

En el desarrollo de formularios con PML existen dos tipos principales de grids o tablas para mostrar y editar datos:

| Tipo              | Tecnología | Edición | Personalización | Uso recomendado |
|-------------------|------------|---------|------------------|------------------|
| `TABLE`           | PML1       | Limitada| Baja             | Formularios simples o lectura |
| `NETGRIDCONTROL`  | PML2 (.NET)| Completa| Alta             | Formularios interactivos, edición avanzada |

---

## 🧱 1. `TABLE` – Grid básica (PML1)

Se define con:

```pml
TABLE .grid WIDTH 60 HEIGHT 15
```

### 🔹 Propiedades comunes

| Propiedad         | Descripción                                            |
| ----------------- | ------------------------------------------------------ |
| `WIDTH`, `HEIGHT` | Dimensiones del componente                             |
| `CALLBACK`        | Ejecuta un método cuando se hace acción sobre la tabla |

### 🔹 Métodos útiles

| Método                  | Función                   |
| ----------------------- | ------------------------- |
| `SetColumns(!array)`    | Define los encabezados    |
| `AppendRow(!array)`     | Añade una fila            |
| `Clear()`               | Elimina todo el contenido |
| `SetCell(x, y, val)`    | Escribe valor en celda    |
| `SetTooltip(x, y, txt)` | Muestra ayuda contextual  |

### 🧪 Ejemplo mínimo funcional

```pml
MEMBER .grid IS TABLE

DEFINE METHOD .INIT()
    !cols = OBJECT ARRAY()
    !cols.Append('Nombre')
    !cols.Append('Edad')
    !cols.Append('Activo')
    !this.grid.SetColumns(!cols)

    !fila = OBJECT ARRAY()
    !fila.Append('Juan')
    !fila.Append(35)
    !fila.Append(TRUE)
    !this.grid.AppendRow(!fila)
ENDMETHOD
```

---

## 🧩 2. `NetGridControl` – Grid avanzada con .NET (PML2)

`NetGridControl` es un control .NET que permite crear grids modernas: filtrables, editables, con validación y compatibles con el entorno AVEVA E3D.

---

## ⚙️ Requisitos

- Usar E3D 2.x o 3.x
- Ejecutar el formulario desde entorno gráfico
- Asegurarse que `GridControl.dll` esté disponible

---

## ⚡ Código mínimo funcional con `NetGridControl`

```pml
SETUP FORM !!GridEjemploNet

    IMPORT 'GridControl'
    HANDLE ANY
    ENDHANDLE
    USING NAMESPACE 'Aveva.Core.Presentation'

    MEMBER .gridCtrl IS NETGRIDCONTROL

    FRAME .zona 'Datos'
        CONTAINER .gridFrame PMLNETCONTROL 'grid' DOCK FILL WIDTH 80 HEIGHT 20
    EXIT

EXIT

DEFINE METHOD .GridEjemploNet()
    IMPORT 'GridControl'
    HANDLE ANY
    ENDHANDLE
    USING NAMESPACE 'Aveva.Core.Presentation'

    !this.gridCtrl = OBJECT NETGRIDCONTROL()
    !this.gridFrame.Control = !this.gridCtrl.Handle()

    !cols = OBJECT ARRAY()
    !cols.Append('Nombre')
    !cols.Append('Edad')
    !cols.Append('Departamento')

    !fila1 = OBJECT ARRAY()
    !fila1.Append('Ana')
    !fila1.Append('29')
    !fila1.Append('HVAC')

    !fila2 = OBJECT ARRAY()
    !fila2.Append('Marcos')
    !fila2.Append('41')
    !fila2.Append('Piping')

    !fila3 = OBJECT ARRAY()
    !fila3.Append('Lucía')
    !fila3.Append('35')
    !fila3.Append('Instrumentación')

    !datos = OBJECT ARRAY()
    !datos.Append(!fila1)
    !datos.Append(!fila2)
    !datos.Append(!fila3)

    !nds = OBJECT NETDATASOURCE('Empleados', !cols, !datos)
    !this.gridCtrl.BindToDataSource(!nds)

    !this.gridCtrl.EditableGrid(TRUE)
    !this.gridCtrl.Updates(TRUE)
    !this.gridCtrl.columnExcelFilter(TRUE)
    !this.gridCtrl.autoFitColumns()

ENDMETHOD

DEFINE METHOD .OnBeforeCellUpdate(!args IS ARRAY)
    $* args = [1]=nuevoValor, [2]=rowTag, [3]=columnKey, [4]=valido?, [5]=mensajeError
    !args[4] = TRUE
ENDMETHOD
```

---

## 🎓 Explicación detallada

### Estructura base

- `SETUP FORM`: define el formulario principal.
- `IMPORT`, `HANDLE`, `USING`: cargan la librería .NET y preparan el entorno.
- `MEMBER .gridCtrl`: define el control como atributo reutilizable.
- `CONTAINER`: espacio visual donde se incrusta el grid .NET.

### Creación de datos

- Se crean arrays para columnas y filas.
- Se compilan en un `NetDataSource`, que actúa como fuente de datos para el control.

### `BindToDataSource`

Este método vincula la tabla visual al contenido creado dinámicamente:

```pml
!nds = OBJECT NETDATASOURCE('Empleados', !cols, !datos)
!this.gridCtrl.BindToDataSource(!nds)
```

Esto:
- Genera las columnas automáticamente
- Rellena filas con contenido
- Establece la estructura base del grid

Si se omite, la grid estará vacía.

### Propiedades visuales

| Método                       | Descripción                               |
|-----------------------------|-------------------------------------------|
| `EditableGrid(TRUE)`        | Permite editar directamente               |
| `Updates(TRUE)`             | Marca cambios para procesarlos            |
| `columnExcelFilter(TRUE)`   | Activa filtros por columna                |
| `autoFitColumns()`          | Ajusta el ancho de las columnas           |

---

## 🔄 Comparativa visual

| Característica             | `TABLE` (PML1) | `NetGridControl` (PML2)         |
|---------------------------|----------------|----------------------------------|
| Edición                   | Básica         | Completa                        |
| Tipos de celda            | Texto          | Texto, checkbox, combobox       |
| Filtros                   | No             | Sí (tipo Excel)                 |
| Eventos                   | Limitados      | Múltiples (`BeforeCellUpdate`)  |
| Visualización             | Limitada       | .NET profesional                |
| Requiere .NET             | No             | Sí (`IMPORT 'GridControl'`)     |
| Recomendado para          | Formularios simples | Interfaces complejas       |

---

## ✅ Conclusión

Usa `TABLE` cuando necesites mostrar pocas filas y sin edición compleja.  
Usa `NetGridControl` cuando:

- Quieras permitir edición por celda
- Necesites filtros por columna
- Requieras una mejor experiencia visual
- Quieras validar cambios antes de aceptarlos

Este capítulo ofrece la base práctica y explicada para trabajar con cualquier tipo de grid en PML. En el próximo capítulo aprenderemos a expandir el uso de grids con funcionalidades avanzadas como exportación a Excel, combobox por columna, y carga desde el modelo 3D.

