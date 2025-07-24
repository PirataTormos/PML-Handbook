# 05 - Gadgets de Formularios en PML1

Los **gadgets** son componentes gráficos usados en formularios `.pmlfrm` para construir interfaces en AVEVA. Aquí se explican los más comunes, con propiedades, métodos y ejemplos.

---

## 🧩 1. TEXT

Campo que puede actuar como etiqueta o como entrada editable si se configura con `IS STRING`.

### Propiedades:
- `TEXT`, `WIDTH`, `IS STRING`
- `VALIDATECALL '...'` (obsoleto en PML2, ver abajo)

### Métodos:
- `!this.texto.val`
- `!this.texto.Clear()`

### Ejemplo:
```pml
TEXT .mensaje 'Título:' WIDTH 30 IS STRING
```

🔗 [Validación con TEXT - pdmsmacro.wordpress.com](https://pdmsmacro.wordpress.com/2013/03/15/validating-input-to-text-fields/?utm_source=chatgpt.com)

---

## 🔘 2. BUTTON

Botón que ejecuta una acción.

### Propiedades:
- `WIDTH`, `CALLBACK '...'`

### Métodos:
- `Enable()`, `Disable()`

### Ejemplo:
```pml
BUTTON .procesar 'Procesar' WIDTH 20 CALLBACK '!this.procesarDatos()'
```

---

## 📝 3. INPUT

Entrada de texto de una sola línea.

### Propiedades:
- `IS STRING`, `IS REAL`
- `VALIDATECALL '...'` (no recomendado en PML2)

### Métodos:
- `val`, `Clear()`, `Enable()`, `Disable()`

### Ejemplo:
```pml
INPUT .usuario WIDTH 25 IS STRING
```

---

## 🧪 VALIDACIÓN EN PML2: CALLBACK EN VEZ DE VALIDATECALL

En **PML2**, se recomienda usar el método `CALLBACK` directamente para validar y procesar entradas, evitando `VALIDATECALL`.

### Ejemplo:
```pml
TEXT .ident 'Check Ident' CALLBACK '!this.checkident()' WIDTH 18 IS STRING
```

```pml
DEFINE METHOD .checkident()
   !!Findident(!this.ident.val)
ENDMETHOD
```

Esto permite acceder directamente al valor con `.val` y manejar la lógica completa dentro del método, de forma más limpia y mantenible.

---

## 📋 4. NUMERIC INPUT

Solo acepta números reales.

```pml
INPUT .monto WIDTH 15 IS REAL
```

---

## 📋 5. LIST

Lista de selección única.

### Métodos:
- `Append()`, `Clear()`, `Count()`, `val`

```pml
LIST .tipos WIDTH 20
```

---

## 📦 6. CHOICE (COMBOBOX)

Menú desplegable.

### Métodos:
- `Append()`, `Clear()`, `val`, `SetIndex()`

```pml
CHOICE .formatos 'Formato:' WIDTH 20
```

---

## 🔘 7. TOGGLE

Interruptor (ON/OFF).

### Métodos:
- `Set(TRUE/FALSE)`, `val`

```pml
TOGGLE .activar 'Modo Avanzado' WIDTH 25
```

---

## 📏 8. LINE

Línea divisoria visual.

```pml
LINE .sep WIDTH 100
```

---

## 🧱 9. FRAME

Agrupa gadgets con `PATH`, `VDIST`, `HDIST`.

```pml
FRAME .grupo 'Datos'
    INPUT .nombre WIDTH 20
EXIT
```

---

## 🔍 10. VIEW

Vista para mostrar modelos o imágenes.

```pml
VIEW .vista3D WIDTH 80 HEIGHT 40
```

🔗 [Documentación oficial - AVEVA](https://docs.aveva.com/bundle/engineering/page/1027223.html?utm_source=chatgpt.com)

---

## 📊 11. TABLE

Tabla de datos.

### Métodos:
- `AppendRow()`, `Clear()`, `SetCell(x,y,v)`
- `SetSize()`, `SetColumns()`, `SetTooltip()`

```pml
TABLE .grid WIDTH 60 HEIGHT 15
```

🔗 [TABLE - AVEVA Docs](https://docs.aveva.com/bundle/engineering/page/1027223.html?utm_source=chatgpt.com)

---

## 🌳 12. TREE

Muestra jerarquías de elementos.

```pml
TREE .estructura WIDTH 40 HEIGHT 30
```

---

## ⚙️ Propiedades comunes

- `WIDTH`, `HEIGHT`, `CALLBACK '...'`
- `VISIBLE`, `DISABLED`, `AT XMIN.Y`

---

## ✅ Recomendaciones

- Declara siempre `MEMBER` para gadgets de entrada.
- Usa `CALLBACK '!this.metodo()'` para control interno.
- Usa `FRAME` para organización clara.

---

Este listado es extensible. Puedes sugerir nuevos gadgets o aportar ejemplos para mejorarlo.