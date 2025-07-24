
# 03 - Funciones en PML

## 📌 ¿Qué es una función?

Una función en PML permite encapsular lógica reutilizable. Se define en archivos `.pmlfnc` y puede recibir argumentos, ejecutar cálculos o lógica, y devolver un resultado con `RETURN`.

Son una herramienta esencial para estructurar el código y evitar duplicación.

---

## 📄 Sintaxis básica

```pml
DEFINE FUNCTION !!NombreFuncion(!param1 IS REAL, !param2 IS REAL) IS REAL
   !resultado = !param1 + !param2
   RETURN !resultado
ENDFUNCTION
```

El nombre del archivo debe coincidir con la función que contiene.  
Ejemplo: `Suma.pmlfnc` contiene `!!Suma(...)`.

---

## 🧪 Tipado de argumentos

### 📘 ¿Qué significa `IS BOOL` (o `IS <tipo>`)?

La parte `IS BOOL` indica el tipo de **valor que devuelve** la función al usar `RETURN`.

Por ejemplo:

```pml
DEFINE FUNCTION !!EsMayorA10(!valor IS REAL) IS BOOL
```

Esto indica que la función devuelve un `BOOLEAN` (`TRUE` o `FALSE`).

> ✅ Si la función **no devuelve ningún valor**, se puede omitir `IS <tipo>`.


PML permite declarar tipos en los argumentos y en el retorno:

```pml
DEFINE FUNCTION !!EsMayorA10(!valor IS REAL) IS BOOL
   IF (!valor GT 10) THEN
      RETURN TRUE
   ELSE
      RETURN FALSE
   ENDIF
ENDFUNCTION
```

---

## ↩️ Devolver resultados

- Se usa `RETURN valor` (sin paréntesis).
- Es posible devolver cualquier tipo: `STRING`, `REAL`, `BOOLEAN`, `ARRAY`, etc.
- Si no se desea devolver nada, se puede omitir el `RETURN`.

---

## 📦 Usar funciones desde macros o formularios

Desde macro:
```pml
!resultado = !!CalcularArea(10, 3)
$P 'Área: ' + $!resultado
```

Desde botón:
```pml
CALL '!!MiFuncion(5)'
```

---

## ⚠️ Validación de entrada

Puedes validar manualmente si un argumento fue pasado:

```pml
IF UNSET(!valor) THEN
   RETURN 'Error: valor no definido'
ENDIF
```

---

## 🔄 Ejemplo completo

Archivo: `Multiplica.pmlfnc`
```pml
DEFINE FUNCTION !!Multiplica(!a IS REAL, !b IS REAL) IS REAL
   IF UNSET(!a) OR UNSET(!b) THEN
      RETURN 0
   ENDIF
   RETURN !a * !b
ENDFUNCTION
```

---

## ✅ Buenas prácticas

- Usa nombres descriptivos: `CalculaVolumen`, `FiltraPorTipo`.
- Declara tipos de entrada y retorno para claridad.
- Valida entradas con `UNSET()` si pueden estar vacías.
- Devuelve siempre un valor consistente.
- Declara una sola función por archivo `.pmlfnc`.
- Usa `!!NombreFuncion` para evitar conflictos con variables locales (`!`).

---


