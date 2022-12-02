# Guía de estilo de codificación
Última actualización: 25 de noviembre de 2023.

La presente guía de estilo indica los aspectos, convenciones y características
que  los programadores, desarrolladores de código y scripters deben tener en
cuenta al escribir código para cualquier proyecto de Tempo Enterprise.

## Lenguaje Pawn

Cuando escribas código en el lenguaje Pawn, por favor, ten presente las siguientes
convenciones:

### De forma general

- **Estilo de identación y espacios en blanco**
    - Corchetes en una nueva línea para absolutamente *todo* (estilo Allman).
    - 4 espacios de identación (no tabulación) para cada nuevo nivel o bloque.
    - Sin identación para `public`, `forward`, funciones `stock` y variables globales.
    - Sin espaciado extra entre parentesis; no hagas `( esto )`.
    - Sin espacio después del nombre de las funciones; un (1) espacio después de
    `if`, `for` y `while`.
    - Si un `if` solo tiene una declaración de una sola linea, puede aparecer en
    la misma línea del `if`, sin corchetes. En todo los demás casos, los corchetes
    son requeridos, y la sentencia `else` debe aparecer correctamente identados en
    una nueva línea.
    - No hay un límite rígido en las longitudes de una línea, sin embargo, prefiere
    mantener lineas < 100 carácteres con el fin de mejorar la legibilidad.

- **Nombramiento**.

    - El nombre de las variables deben ser en inglés y _camelcase_ (myVariable).
        - Las variables globales deben tener `g` o `_` como prefijo.
        - Las variables de módulos deben tener el nombre del módulo como prefijo.
    - Los nombres de constantes en tiempo de compilación deben ser totalmente 
    _uppercase_, y usar un `_` para separar las palabras.
    - Los nombres de las funciones deben ser UpperCamelCase (PascalCase).
        - Los nombres de las funciones de módulos deben tener el nombre del módulo
        (con la primera letra en mayúscula) como prefijo (Missions_g_sMissionLevel).
    - El nombre de los `#define` y los `enum` debe ser en uppercase y usar un `_` para separar las palabras.
        - Los `enum` prioritario o de mayor peso deben usar `E_` como prefijo.
        - Los `enum` secundarios o de menor peso deben usar `e_` como prefijo.
    - El nombre de las variables, funciones y constantes no debe superar los 30
    carácteres.

- **Misceláneos**

    - Proximamente...

Código de bloque de ejemplo:

```cpp
new
        g_BuildNumber = 0,
        mainString[12],
        ocassionalTour,
Float:  areaSquare = 0.0;

public OnPlayerWalk(playerid, n)
{
    for (new i = 0; i < n; ++i) 
    {
        new totalSum = 0;

        if (!Something()) return false;
        ...
        if (SomethingElse(i)) 
        {
            totalSum += CalculateSomething(ocassionalTour);
        } 
        else 
        {
            DoSomething(areaSquare, totalSum);
        }
    }

    // Success return is usually at the end
    return true;    
}
```

### Modularidad

Todos los proyectos realizados en Tempo Enterprise deben ser modulares. Es decir,
subdividirse en componentes independientes; ir de lo general a los particular. Las
siguientes son las convenciones para escribir código de forma modular:

 - Se debe determinar la posición raiz del módulo, previo a su creación. Si es 
 necesario para la operatividad del modo de juego, se debe incluir en la rama 
 `core`. De lo contrario, se debe incluir en alguna otra rama donde encaje.
 - Si es creado dentro de la raiz `core`, todos los archivos de este módulo deben
 ir en nueva carpeta. El nombre de la carpeta debe ser un sustantivo en inglés que
 represente e identifique claramente al módulo, escrito en minusculas, en singular
 y multiples palabras separadas por guión (`-`). 
   - Por ejemplo: `/core/admin`, `/core/user-interface` o `/core/house`. Submodulos
   deben seguir la misma convención para su nombre. Por ejemplo: `/core/player/job/`
   o `/core/house/furniture`.
 - El archivo principal, que contiene el inicializador de este módulo, debe llevar
 el mismo nombre del modulo. Por ejemplo, `/core/chat/chat.inc`.
 - Los archivos que componen este módulo (en adelante **clases**), deben llevar un 
 préfijo con el nombre del módulo en minúscula y separado por un guión bajo (`_`) 
 del nombre de la clase. El nombre de la clase se rige bajo la misma convención 
 para nombrar al módulo (si el nombre del módulo es multipalabras, se debe abreviar).
   - Por ejemplo: `/core/player/player_job.inc`, `/core/player/attachment/attachment_setup-table.inc`
   o `/core/user-interface/ui_connect-message.inc`.


**NOTA:** No se abrevia, resume o simplifica ningún nombramiento de módulos o 
variables, excepto en los casos que se indica. Verificar la escritura y ortografía
de la palabra.

**Definiendo una nuevo módulo**

Todas los módulos deben comenzar con una protección de doble apertura con la siguiente
estructura:

```cpp
#if defined _PARENT_LIBRARY_included
    #endinput
#endif
#define _PARENT_LIBRARY_included
```

Asegurarse que la definición `_PARENT_LIBRARY_included` no exceda los 32 carácteres.
Por ejemplo:
```cpp
#if defined _core_server_included
    #endinput
#endif
#define _core_server_included
```

Si la carpeta padre y el módulo tienen el mismo nombre, se puede mencionar una sola
vez. Es decir, en lugar de `_utils_utils_included` solo se menciona `_utils_included`

Solo se protege el módulo principal en una carpeta raiz, las sublibrerías no se 
protegen.

### Comentarios
Los comentarios son parte importante de la programación colaborativa. Es importante
que describas el uso de funciones, variables y métodos tan eficaz como sea posible.
Los comentarios deben ser redactados en inglés y como se muestra a continuación.

Por ejemplo, para describir una función usa:

```cpp
/**
 * ... Description ...
 *
 * @param[in]  arg1 input description...
 * @param[in]  arg2 input description...
 * @param[out] arg3 output description...
 * @return Return cases...
 * @throws Error type and cases...
 * @pre  Pre-condition for function...
 * @post Post-condition for function...
 */

public function(arg1, const arg2, String:&arg3)
```

En general, para describir componentes de tu código usa:

```cpp
/**
 * Alerts are for notifying old versions if they become too obsolete and
 * need to upgrade. The message is displayed in the status bar.
 * @see GetWarnings()
 */
 stock Alert();
```

Para describir una variable usa:

```cpp
/// Description before the var
new var;
```
o
```cpp
new var; /// Description in same line as the var
```

Eres bienvenido de hacer contribuciones directamente por medio de una pull request.
Asegúrate de hacer primero un issue de tu propuesta.