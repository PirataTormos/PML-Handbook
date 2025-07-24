# 01 - Control de flujo en PML

## 🧭 ¿Qué es el control de flujo?

El control de flujo en PML permite definir cómo se ejecuta el código: tomar decisiones, repetir bloques o manejar excepciones. Aunque PML2 es orientado a objetos, mantiene muchas construcciones tradicionales heredadas de PML1.

---

## 🔁 Condicionales (`IF`, `THEN`, `ELSE`, `ELSEIF`, `ENDIF`)

Permiten ejecutar bloques solo si se cumple una condición:

```pml
!x = 10
IF (!x GT 0) THEN
   !!Alert.Message('Valor positivo')
ELSE
   !!Alert.Message('Valor negativo o cero')
ENDIF
```

También puedes usar múltiples condiciones:

```pml
IF (!x EQ 1) THEN
   $P 'Es uno'
ELSEIF (!x EQ 2) THEN
   $P 'Es dos'
ELSE
   $P 'No es ni uno ni dos'
ENDIF
```

> **Notas:**
> - Los operadores de comparación son: `EQ`, `NE`, `LT`, `LE`, `GT`, `GE`.
> - No se usan paréntesis, pero son permitidos.
> - Siempre se requiere `THEN` y `ENDIF`.

---

## 🔂 Bucles (`DO`, `ENDDO`)

### Bucle con contador:

```pml
DO !i FROM 1 TO 5 BY 1
   $P 'Iteración ' + $!i
ENDDO
```

### Bucle sobre colecciones:

```pml
!nombres = object ARRAY()
!nombres.Append('Ana')
!nombres.Append('Luis')

DO !nombre VALUES !nombres
   !!Alert.Message('Hola, ' + !nombre)
ENDDO
```

### Sentencias internas: `BREAK`, `SKIP`

```pml
DO !i FROM 1 TO 10
   IF (!i EQ 5) THEN
      SKIP  -- salta esta iteración
   ENDIF

   IF (!i GT 8) THEN
      BREAK  -- termina el bucle
   ENDIF

   $P 'Valor: ' + $!i
ENDDO
```

- `SKIP` / `SKIP IF (<cond>)`: omite el resto del bloque y pasa a la siguiente iteración.
- `BREAK` / `BREAK IF (<cond>)`: finaliza el bucle inmediatamente.

---

## 🔁 GOLABEL y GOTO

Permiten saltos directos (poco recomendados):

```pml
inicio:
  !!Alert.Message('Ciclo infinito')
  GOLABEL inicio
```

> ⚠️ **Evitar uso innecesario.** PML2 permite estructuras más claras y mantenibles.

---

## ⚠️ Validaciones comunes

### BADREF

Evalúa si una referencia a un elemento del modelo es válida:

```pml
!e = /SITE1/ZONE1/EQ01
IF BADREF(!e) THEN
   !!Alert.Message('Elemento no existe')
ELSE
   !!Alert.Message('Elemento válido: ' + !e.Name)
ENDIF
```

### UNSET

Verifica si una variable tiene valor definido:

```pml
IF UNSET(!v) THEN
   !!Alert.Message('Variable no inicializada')
ENDIF
```

---

## 🧪 Control de errores (`HANDLE`)

### Capturar errores específicos:

```pml
HANDLE (160,9)
   !!Alert.Message('Fichero no encontrado')
ENDHANDLE
```

### Capturar cualquier error:

```pml
HANDLE ANY
   !!Alert.Message('Error genérico capturado')
ENDHANDLE
```

### Capturar múltiples errores diferenciados:

```pml
HANDLE (100)
   !!Alert.Message('Error 100 capturado')
ELSEHANDLE (101)
   !!Alert.Message('Error 101 capturado')
ELSEHANDLE (ANY)
   !!Alert.Message('Otro error capturado')
ENDHANDLE
```

> Útil para errores de lectura de archivos, acceso a BD, operaciones de red o condiciones de validación.

---

## ✅ Buenas prácticas

- Usa `IF` con `THEN` y `ENDIF` siempre, incluso en bloques de una sola línea.
- Prefiere bucles `DO ... VALUES` para iterar arrays.
- Valida con `BADREF` antes de acceder a propiedades de elementos del modelo.
- Usa `UNSET` para evitar errores con variables opcionales.
- Usa `HANDLE` con paréntesis. Considera `HANDLE ANY` para errores genéricos.
- Controla bucles con `BREAK` y `SKIP` para mayor flexibilidad.

---