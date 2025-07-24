# 06 - Métodos y Members en PML2

Los **métodos** permiten encapsular lógica dentro de formularios u objetos en PML2. Funcionan como funciones asociadas al contexto donde se declaran. Los **members** son atributos propios del formulario u objeto y actúan como variables persistentes entre métodos.

---

## 🔹 1. ¿Qué es un método?

Un **método** es una rutina que define el comportamiento de un formulario u objeto. Se declara con `DEFINE METHOD .nombre()` y se accede con `!this.nombre()` o desde fuera con `!!objeto.nombre()`.

---

## 🔹 2. ¿Qué es un MEMBER?

Un **member** es una variable que pertenece al formulario u objeto donde se declara. Puede ser accedida desde cualquier método interno usando `!this.<nombre>`. Se declara así:

```pml
MEMBER .datos IS ARRAY()
```

Y se accede así:
```pml
!this.datos.Append('Elemento')
```

### ✅ Características clave:
- Persisten mientras el formulario esté activo.
- Son equivalentes a atributos en programación orientada a objetos.
- Su uso facilita encapsulación y modularidad.
- Se pueden declarar tipos complejos como `ARRAY`, `REAL`, `STRING`, etc.

**Ejemplo completo:**
```pml
MEMBER .contador IS REAL

DEFINE METHOD .incrementar()
   !this.contador = !this.contador + 1
ENDMETHOD
```

---

## 🔹 3. Estructura de un método

```pml
DEFINE METHOD .procesar()
   $P 'Procesando...'
ENDMETHOD
```

> **Importante**: Los métodos deben finalizar con `ENDMETHOD`.

---

## 🔹 4. Llamar métodos desde gadgets

Para ejecutar un método desde un botón:
```pml
BUTTON .enviar 'Enviar' CALLBACK '!this.procesar()'
```

El método se ejecutará cuando el usuario pulse el botón.

---

## 🔹 5. Métodos especiales del ciclo de vida

| Método             | Descripción                                               |
|--------------------|-----------------------------------------------------------|
| `.INIT()`          | Se ejecuta al abrir el formulario por primera vez         |
| `.close()`         | Se ejecuta al cerrarse el formulario                      |
| `.firstShownCall()`| Primera vez que se muestra                                |
| `.killingCall()`   | Al eliminar el formulario                                 |

**Ejemplo:**
```pml
DEFINE METHOD .INIT()
   $P 'Formulario iniciado'
ENDMETHOD
```

---

## 🔹 6. Métodos en objetos

```pml
DEFINE OBJECT PERSONA
   MEMBER .Nombre IS STRING
ENDOBJECT

DEFINE METHOD .SetNombre(!nombre IS STRING)
   !this.Nombre = !nombre
ENDMETHOD
```

Y su uso:
```pml
!p = OBJECT PERSONA()
!!p.SetNombre('Ana')
```

---

## 🔹 7. Buenas prácticas

- Usa `CALLBACK '!this.metodo()'` siempre en gadgets.
- Coloca lógica en métodos separados del constructor.
- Usa `!this.` para todo acceso interno.
- Prefiere `MEMBER` frente a variables globales o externas.

---

## 🔹 8. Errores comunes

- Usar `!!` en vez de `!this` dentro del formulario
- Olvidar `ENDMETHOD`
- Llamar el método sin comillas: ❌ `CALLBACK !this.metodo()`

---

## 🔹 9. Métodos implícitos útiles

| Método           | Tipo de objeto    | Ejemplo                          |
|------------------|-------------------|----------------------------------|
| `.val`           | INPUT, LIST       | `!this.nombre.val`               |
| `.Append()`      | ARRAY, LIST       | `!this.lista.Append('X')`        |
| `.Clear()`       | ARRAY, gadgets    | `!this.array.Clear()`            |
| `.Count()`       | ARRAY, LIST       | `!this.array.Count()`            |
| `.UpCase()`      | STRING            | `!mayus = !texto.UpCase()`       |
| `.Replace()`     | STRING            | `!nuevo = !txt.Replace('x','y')` |

---

## 🔗 Fuentes y documentación

- [PML Básico - studylib.net](https://studylib.net/doc/27098831/pml-basics?utm_source=chatgpt.com)
- [Diseño de formularios - scribd.com](https://www.scribd.com/document/491334486/Material-PML-Training-Form-Design-REV06?utm_source=chatgpt.com)
- [AVEVA Engineering Help](https://help.aveva.com/AVEVA_Everything3D/1.1/SOFTCG/wwhdata/files.htm?utm_source=chatgpt.com)

---

¿Te gustaría proponer más métodos típicos o incorporar fragmentos reales?