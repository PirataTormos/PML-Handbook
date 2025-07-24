
# 02 - Macros en PML

## 📄 ¿Qué es una macro?

Una **macro PML** es un archivo de texto con extensión `.pmlmac` que contiene una secuencia de instrucciones ejecutables. Se utiliza para automatizar tareas, procesar datos, controlar elementos del modelo o lanzar interfaces gráficas.

Las macros son la unidad básica de ejecución en PML, especialmente en PML1.

---

## ▶️ Cómo ejecutar una macro

### Opción 1: Usando el nombre (si se configura `MACROPATH`)

```pml
$M MiMacro
```

> Busca y ejecuta el archivo `MiMacro.pmlmac` en las carpetas definidas en el entorno (variable `MACROPATH`).

### Opción 2: Usando ruta completa o relativa

```pml
$M/\D:\MisMacros\ToolA\MiMacro
```

> Ejecuta la macro desde una ubicación específica.  
> Esta es la forma más común si no se ha configurado `MACROPATH`.  
> ⚠️ Importante: se debe anteponer `/` a la ruta completa.

### Desde otra macro:
```pml
$M OtraMacro
```

### Desde un botón de formulario:
```pml
BUTTON .btnRun AT X1 Y1 TEXT |Ejecutar| CALL |$M MiMacro|
```

---

## 🧱 Estructura básica

```pml
-- MiPrimerMacro.pmlmac

$P 'Inicio de la macro'

!a = 5
!b = 10
!suma = !a + !b

$P 'Resultado: ' + $!suma
```

---

## 🧮 Variables y lógica

Las macros pueden contener:

- Declaraciones (`!var`, `!!var`)
- Operadores aritméticos y lógicos
- Condicionales (`IF`, `ELSE`)
- Bucles (`DO`)
- Saltos (`GOLABEL`, `GOTO`)
- Llamadas a funciones (`!!MiFuncion(...)`)

---

## 📬 Llamar funciones desde macros

Si tienes una función definida en un archivo `.pmlfnc`:

```pml
-- Archivo: CalcArea.pmlfnc
DEFINE FUNCTION !!CalcArea(!base, !altura)
   RETURN (!base * !altura)
ENDDEFINE
```

Puedes llamarla desde una macro:

```pml
!area = !!CalcArea(10, 5)
$P 'Área: ' + $!area
```

---

## ⚠️ Errores comunes

- ❌ Llamar una macro sin que esté en la ruta correcta.
- ❌ Usar métodos `.String()` dentro de `$P` → no funciona en PML1.
- ❌ Usar variables no inicializadas → usar `UNSET()` para validar.

---

## ✅ Buenas prácticas

- Usa nombres descriptivos: `RenombraEquipos.pmlmac`
- Documenta al principio qué hace la macro.
- No abuses de `GOLABEL`; prefiere estructuras claras.
- Agrupa lógicamente en secciones: entrada, procesamiento, salida.

---


