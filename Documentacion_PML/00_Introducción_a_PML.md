
# 00 - Introducción a PML

## 🌐 ¿Qué es PML?

PML (Programmable Macro Language) es el lenguaje de scripting utilizado en AVEVA PDMS y AVEVA E3D para automatizar tareas, crear interfaces gráficas y extender la funcionalidad del entorno de diseño. Se integra profundamente con la base de datos de PDMS/E3D y permite desarrollar herramientas personalizadas para modelado, administración y control de calidad.

Existen dos versiones principales:

- **PML1**: versión original basada en macros de texto plano. Se usa principalmente en plantillas, reglas y scripts heredados.
- **PML2**: versión orientada a objetos, con sintaxis estructurada y tipos nativos. Es la recomendada para nuevos desarrollos.

---

## 🗋 Tipos de archivos en PML

| Extensión   | Propósito                                 |
|-------------|--------------------------------------------|
| `.pmlmac`   | Macro clásica. Ejecutable con `$M archivo` |
| `.pmlfnc`   | Funciones definidas con `define function`  |
| `.pmlobj`   | Objetos con atributos y métodos propios    |
| `.pmlfrm`   | Formularios gráficos definidos con `setup form` |

**⚠️ Importante:** El nombre del archivo debe coincidir exactamente con el nombre de la función, objeto o formulario que contiene.  
Ejemplos:
- `CalcArea.pmlfnc` → debe contener `define function !!CalcArea(...)`.
- `MyForm.pmlfrm` → debe contener `SETUP FORM !!MyForm`.

---

## 📊 Variables y ámbito

- `!var`: variable local. Solo existe dentro del método o macro.
- `!!var`: variable global. Persiste durante toda la sesión.

Ejemplos:
```pml
!temp = 100             -- variable local
!!UserName = 'Tormos'   -- variable global
```

---

## 🧰 Tipos de datos nativos

| Tipo     | Descripción                        | Ejemplo                             |
|----------|------------------------------------|-------------------------------------|
| `REAL`   | Número decimal                     | `!x = 3.14`                         |
| `STRING` | Texto                              | `!s = 'Hola mundo'`                 |
| `BOOLEAN`| Valor lógico (`TRUE` o `FALSE`)    | `!ok = TRUE`                        |
| `DBREF`  | Referencia a elemento del modelo   | `!e = /SITE1/ZONE1/EQ1`             |
| `ARRAY`  | Colección indexada de elementos    | `!a = object ARRAY(); !a.Append(1)` |

---

## 🧹 Operadores

### Comparación
- `EQ`, `NE`, `LT`, `LE`, `GT`, `GE`

### Lógicos
- `AND`, `OR`, `NOT`

### Aritméticos
- `+`, `-`, `*`, `/`

### Concatenación de strings
```pml
!nombre = 'Juan'
!saludo = 'Hola ' + !nombre
```

---

## 📝 Comentarios y comandos especiales

- Comentarios:
  - `-- Esto es un comentario`
  - `$* Comentario multilinea`

- Mostrar mensaje por consola (PML1):
  ```pml
  $P 'Mensaje a mostrar'
  $P 'Valor: ' + $!valor
  ```
  > ⚠️ **Importante:** `$P` es una sentencia de PML1. Solo admite referencias simples como `$!var`, pero **no permite usar métodos** como `.String()`. Este es un error frecuente.

- Ejecutar macro:
  ```pml
  $M mi_macro.pmlmac
  ```

- Depurar (mostrar trazas):
  ```pml
  $R1                          -- Muestra traza
  !!Alert.Message('Texto')    -- Alternativa moderna
  ```

---

## 📌 Buenas prácticas

- Prefiere **PML2** para nuevos desarrollos.
- Usa `!this.<miembro>` en formularios y objetos para mantener encapsulación.
- Separa la lógica en funciones y objetos reutilizables.
- Declara variables de estado como `MEMBER` si se usarán en métodos del mismo formulario.
- Comenta claramente cada sección del código.
- Asegúrate de que **el nombre del archivo coincida con el objeto o función que contiene.**

---

