# 04 - Formularios en PML1: estructura práctica

## 🧱 ¿Qué es un formulario en PML?

Un formulario (`.pmlfrm`) es una interfaz gráfica definida con **PML1**. Es el principal medio para construir herramientas visuales dentro del entorno AVEVA.  
Cada formulario se comporta como un **objeto**, con su propio estado interno (`MEMBERS`) y lógica (`METHODS`).

---

## ⚙️ Estructura real basada en formularios profesionales

```pml
SETUP FORM !!MiFormulario dialog dock right

    -- CONSTRUCTION MEMBERS
    MEMBER .modo IS STRING
    MEMBER .entradaNombre IS STRING
    MEMBER .datos IS ARRAY()

    -- CONSTRUCTOR
    !this.formTitle = 'Mi herramienta' 
    !this.cancelCall = '!this.close()'

    PATH DOWN
    VDIST 0.3
    HDIST 1

    !ancho = 30

    -- INTERFAZ GRÁFICA
    FRAME .principal 'Panel Principal'
        TEXT .label1 'Ingrese nombre:'
        INPUT .entradaNombre WIDTH $!ancho
        BUTTON .procesar 'Procesar' WIDTH $!ancho CALLBACK '!this.procesar()'
    EXIT

    FRAME .utiles 'Herramientas'
        PATH RIGHT
        BUTTON .reload 'Recargar' WIDTH 12 CALLBACK '!this.recargar()'
        BUTTON .cerrar 'Cerrar' WIDTH 12 CALLBACK 'KILL !!MiFormulario'
    EXIT

EXIT
```

---

## 📚 Explicación detallada de la estructura

### 🔹 `SETUP FORM !!MiFormulario`
Inicia la definición del formulario. El nombre `!!MiFormulario` debe coincidir con el nombre del archivo `.pmlfrm`.

El parámetro `dialog dock right` indica que el formulario será acoplable y aparecerá anclado a la derecha.

### 🔹 `MEMBER`
Declara atributos del formulario accesibles desde cualquier método.  
Todos los `MEMBER` se acceden con `!this.<nombre>`. Ejemplos:

- `MEMBER .datos IS ARRAY()` → `!this.datos.Append(...)`
- `!this.formTitle` es un miembro implícito y se usa para mostrar el título del formulario.
- `!this.cancelCall` define qué hacer al pulsar "Cancelar" o cerrar.

### 🔹 `PATH`, `HDIST`, `VDIST`
Controlan la disposición y separación entre elementos visuales:

- `PATH DOWN` → apila controles verticalmente.
- `PATH RIGHT` → organiza de izquierda a derecha.
- `HDIST` y `VDIST` → separación horizontal y vertical, respectivamente.

Ejemplo visual:

```
PATH DOWN:
[Botón A]
[Botón B]
[Texto C]

PATH RIGHT:
[Campo A][Campo B][Campo C]
```

### 🔹 `FRAME`
Bloque que agrupa controles relacionados. Cada uno se debe cerrar con `EXIT`.

---

## 🧠 Métodos y lógica interna

Los botones ejecutan funciones propias del formulario mediante `CALLBACK`.  
Se declaran así:

```pml
DEFINE METHOD .procesar()
    $P 'Procesando: ' + $!this.entradaNombre.STRING
ENDMETHOD
```

### Método especial: `INIT`

Se ejecuta automáticamente al abrir el formulario:

```pml
DEFINE METHOD .INIT()
    !this.datos = OBJECT ARRAY()
    !this.datos.Append('Ejemplo')
ENDMETHOD
```

---

## 👁️ Mostrar y cerrar el formulario

- **Abrir**: `SHOW !!MiFormulario()`
- **Cerrar y reiniciar**: `KILL !!MiFormulario`

> Si se cierra con la "X", solo se oculta; para reiniciarlo hay que hacer `KILL` seguido de `SHOW`.

---

## ✅ Buenas prácticas

- Usa `!this.<member>` en todo acceso interno.
- Declara los `MEMBERS` si se usan en varios métodos.
- Separa la interfaz en `FRAME`s por funcionalidad.
- Toda acción debe ir a un método del formulario.
- Usa `INIT()` para inicializar al cargar.

---
