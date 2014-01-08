# Notas generales sobre CSS, consejos y directrices

---

## Traducciones

* [Russian](https://github.com/matmuchrapna/CSS-Guidelines/blob/master/README%20Russian.md)
* [Chinese](https://github.com/chadluo/CSS-Guidelines/blob/master/README.md)
* [French](https://github.com/flexbox/CSS-Guidelines/blob/master/README.md)
* [Spanish](https://github.com/unavezfui/CSS-Guidelines/blob/master/README.md)


---

Al trabajar en proyectos grandes y de larga duración con docenas de desarrolladores, es importante que todos trabajemos de manera unificada con el fin de, entre otras cosas:

* Dejar las hojas de estilo mantenibles.
* Dejar el código transparente y legible.
* Dejar las hojas de estilo escalables.

Hay una variedad de técnicas que debemos emplear con el fin de satisfacer estos objetivos.

La primera parte de este documento tratará de la sintaxis, formateo y estructura del CSS, la segunda parte tratará del enfoque, perspectiva y actitud hacia la escritura y arquitectura del CSS. Excitante, ¿eh?

## Contenidos

* [Anatomía de un documento CSS](#anatomía-de-un-documento-css)
  * [General](#general)
  * [Un archivo vs. varios archivos](#un-archivo-vs-varios-archivos)
  * [Tabla de contenidos](#tabla-de-contenidos)
  * [Títulos de sección](#títulos-de-sección)
* [Orden del código](#orden-del-código)
* [Anatomía de los conjuntos de reglas.](#anatomía-de-los-conjuntos-de-reglas)
* [Convenciones de nombres](#convenciones-de-nombres)
  * [Anclas JS](#anclas-js)
  * [Internacionalización](#internacionalización)
* [Comentarios](#comentarios)
  * [Comments on steroids](#comments-on-steroids)
    * [Selectores cuasi-calificados](#selectores-cuasi-calificados)
    * [Código de etiquetas](#código-de-etiquetas)
    * [Indicadores de objeto/extensión](#indicadores-de-objeto-extensión)
* [Escribiendo CSS](#escribiendo-css)
* [Construir nuevos componentes](#construir-nuevos-componentes)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Sizing UIs](#sizing-uis)
  * [Font sizing](#font-sizing)
* [Shorthand](#shorthand)
* [IDs](#ids)
* [Selectors](#selectors)
  * [Over qualified selectors](#over-qualified-selectors)
  * [Selector performance](#selector-performance)
* [CSS selector intent](#css-selector-intent)
* [`!important`](#important)
* [Magic numbers and absolutes](#magic-numbers-and-absolutes)
* [Conditional stylesheets](#conditional-stylesheets)
* [Debugging](#debugging)
* [Preprocessors](#preprocessors)

---

## Anatomía de un documento CSS

Independiente del documento, siempre debemos probar y mantener un formato común. Esto significa comentarios consistentes, sintaxis consistente y nomenclatura consistente.

### General

Limita tus hojas de estilos a un máximo de 80 caracteres de ancho donde sea posible. Las excepciones pueden ser la sintaxis de degradados y URLs en comentarios Está bien, no hay nada que podamos hacer al respecto.

Prefiero las indentaciones de cuatro (4) espacios en lugar de los tabuladores y escribir CSS multilínea.

### Un archivo vs. varios archivos

Algunas personas prefieren trabajar con un solo archivo individual, grande. Esto está bien, por señirte a las pautas a continuación no encontrarás problemas. Desde la migración a SASS he comenzado a fragmentar mis hojas de estilo en montones de incluidos diminutos. Esto también está bien... Sea cual sea el método que elijas, las siguientes reglas y directrices se aplican. La única diferencia notable es en relación a nuestra tabla de contenidos y nuestros títulos de secciones. Sigue leyendo para una explicación más detallada...

### Tabla de contenidos

Al principio de las hojas de estilos, mantengo una tabla de contenidos la cual detallará las secciones contenidas en el documento, por ejemplo:

    /*------------------------------------*\
        $CONTENIDOS
    \*------------------------------------*/
    /**
     * CONTENIDOS..........¡Lo estás leyendo!
     * RESET...............Estabecer nuestro reset por defecto
     * FONT-FACE...........Importar archivos de fuentes
     */

Esto dirá a los siguientes desarrolladores exactamente lo que pueden esperar encontrar en este archivo. Cada elemento en la tabla de contenidos se aplica directamente a un título de sección.

Si estás trabajando en una hoja de estilos grande, la sección correspondiente también estará en ese archivo. Si estás trabajando con múltiples archivos entonces cada elemento en la tabla de contenido apuntará a un incluido que traerá esa sección.

### Títulos de sección

La tabla de contenidos no sería de ningún uso a menos que tuviese los correspondientes títulos de sección. Por lo tanto, designemos una sección:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

La `$` prefijando al nombre de la sección nos permite ejecutar una búsqueda ([Cmd|Ctrl]+F) para `$[NOMBRE-DE-SECCIÓN]` y **limitar nuestro ámbito de búsqueda sólo a los títulos de sección**.

Si estás trabajando en una hoja de estilos grande, dejas cinco (5) saltos de línea entre cada sección, así:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Nuestros
    estilos
    de reseteo]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

Este gran trozo de espacios en blanco es rápidamente perceptible cuando se desplaza rápidamente a través de archivos grandes.

Si estás trabajando a través de múltiples hojas de estilo incluidas, comienza cada uno de esos archivos con un título de sección y no hay necesidad de ningún salto de línea.

## Orden del código

Prueba y escribe las hojas de estilo en orden de especificidad. Esto asegura que tomes completa ventaja de la herencia y de la primera C de CSS; la Cascada.

Una hoja de estilo bien ordenada se ordenará de forma parecida a esta:

1. **Reset** – empezamos de cero.
2. **Elements** – `h1` sin clase, `ul` sin clase etc.
3. **Objetos y abstracciones** — genéricos, patrones de diseño subyacente.
4. **Componentes** – Componentes - componentes completos construidos a partir de los objetos y sus extensiones.
5. **Buen estilo** – Estados de error, etc.

Esto significa que -- mientras recorres el documento--cada sección se construye sobre y se hereda sensatamente de las anteriores. Debería haber menos deshacer de estilos, menos problemas de especificidad y hojas de estilo mejor estructuradas en todos los sentidos.

Para lectura adicional no puedo recomendar lo suficiente a [SMACSS](http://smacss.com) de Jonathan Snook.

## Anatomía de los conjuntos de reglas.

    [selector]{
        [propiedad]:[valor];
        [<- Declaración ->]
    }

Tengo un número de estándares cuando estructuro conjuntos de reglas.

* Usa nombres de clases delimitados por guiones [see below](#naming-conventions))
* Indentación de 4 espacios
* Multilínea
* Declaraciones por orden de relevancia (no alfabético)
* Indentar las declaraciones de prefijos para que sus valores estén alineados
* Indentar nuestros conjuntos de reglas para reflejar el DOM
* Incluir siempre el punto y coma al final de un conjunto de reglas

Un breve ejemplo:

    .widget{
        padding:10px;
        border:1px solid #BADA55;
        background-color:#C0FFEE;
        -webkit-border-radius:4px;
           -moz-border-radius:4px;
                border-radius:4px;
    }
        .widget-heading{
            font-size:1.5rem;
            line-height:1;
            font-weight:bold;
            color:#BADA55;
            margin-right:-10px;
            margin-left: -10px;
            padding:0.25em;
        }

Aquí podemos ver que `.widget-heading` debe ser un hijo de `.widget` como hemos indentado el conjunto de reglas de `.widget-heading` un nivel más profundo que `.widget`. Esta es una información útil para los desarrolladores que ahora puede ser obtenida con sólo una mirada a la indentación de nuestros conjuntos de reglas.

También podemos ver que las declaraciones de `.widget-heading` están ordenadas por su relevancia; `.widget-heading` debe de ser un elemento textual así que empezamos con nuestras reglas de texto, seguidas de todo lo demás.

Una excepción para nuestra regla multilínea podría ser en casos de lo siguiente:

    .t10    { width:10% }
    .t20    { width:20% }
    .t25    { width:25% }       /* 1/4 */
    .t30    { width:30% }
    .t33    { width:33.333% }   /* 1/3 */
    .t40    { width:40% }
    .t50    { width:50% }       /* 1/2 */
    .t60    { width:60% }
    .t66    { width:66.666% }   /* 2/3 */
    .t70    { width:70% }
    .t75    { width:75% }       /* 3/4*/
    .t80    { width:80% }
    .t90    { width:90% }

En este ejemplo (del [sistema de cuadrícula de inuit.css](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88)) tiene más sentido poner nuestro CSS en una sola línea.

## Convenciones de nombres

Para la mayor parte simplemente uso clases delimitadas por guiones (p.ej. `.foo-bar`, no `.foo_bar` o `.fooBar`), sin embargo en ciertas circunstancias uso notación BEM (Bloque, Elemento, Modificador).

<abbr title="Block, Element, Modifier">BEM</abbr> es una metodología para denominar y clasificar selectores CSS de forma que se los hace mucho más estrictos, transparentes e informativos.

La convención de nombres sigue este patrón:

    .bloque{}
    .bloque__elemento{}
    .bloque--modificador{}

* `.bloque` representa el nivel más alto de una abstracción o componente.
* `.bloque__elemento` representa un descendiente de `.bloque` que ayuda a formar `.bloque` como un todo.
* `.bloque--modificador` representa un estado o versión diferente de `.bloque`.

Una **anología** de cómo trabajan las clases BEM podría ser:

    .persona{}
    .persona--mujer{}
        .persona__mano{}
        .persona__mano--izquierda{}
        .persona__mano--derecha{}

Aquí podemos ver que el objeto básico que estamos describiendo es una persona, y que un tipo diferente de persona podría ser una mujer. También podemos ver que las personas tienen manos; estas son sub-partes de las personas y hay diferentes variaciones, como izquierda y derecha.

Ahora podemos nombrar nuestros selectores basándonos en sus objetos base, y también comunicar qué trabajo hace el selector; ¿Es un subcomponente (`__`) o una variación (`--`)?

Así, `.page-wrapper` es un selector independiente; no forma parte de una abstracción o un componente y como tal es nombrado correctamente. `.widget-heading`, no obstante, _está_ relacionado con un componente; es un hijo de la estructura `.widget` así que renombraríamos esta clase `.widget__heading`.

BEM parece un poco más feo, y es mucho más verboso, pero nos da mucho poder en la obtención de información sobre las funciones y relaciones de los elementos únicamente con sus clases. Además, la sintaxis BEM se comprimirá (gzip) por lo general muy bien ya que la compresión favorece/funciona bien con la repetición.

Independientemente de si necesitas usar BEM o no, asegúrate siempre de que las clases están nombradas con sensatez; mantenlas lo más cortas posible pero tan largas como sea necesario. Asegúrate de que todos los objetos o abstracciones están muy vagamente nombrados (p.ej. `.ui-list`, `.media`) para permitir una mayor reutilización. Las extensiones de los objetos deberían estar nombradas de forma mucho más explícita (p.ej. `.user-avatar-link`). No te preocupes por la cantidad o longitud de las clases; gzip coprimirá el código bien escrito _increíblemente_ bien.

### Clases en HTML

En un intento por hacer las cosas más fáciles de leer, separa las clases en HTML con dos (2) espacios, de este modo:

    <div class="foo--bar  bar__baz">

Este aumento de los espacios en blanco debería, con suerte, permitir una localización y lectura más fácil de las clases múltiples.

### JS hooks

**Nunca uses una clase de _estilo_ CSS como un ancla de JavaScript.** Asociar un comportamiento JS a una clase de estilo significa que nunca podremos tener uno sin el otro.

Si necesitas agarrarte a alguna anotación, usa una clase CSS específica de JS. Esto es simplemente una clase con una denominación `.js-`, p.ej: `.js-toggle`, `.js-drag-and-drop`. Esto significa que podemos asociar tanto JS como CSS a las clases en nuestra anotación  pero nunca habrá ningún solapado problemático.

    <th class="is-sortable  js-is-sortable">
    </th>

La anotación anterior contiene dos clases; una a la que puedes adjuntar algo de estilo para columnas de tabla clasificables y otra que te permite añadir la funcionalidad de clasificación.

### Internacionalización

A pesar de ser un desarrollador británico —y haber pasado toda mi vida escribiendo "colour" en lugar de "color"— siento que, por el bien de la consistencia, es mejor usar siempre inglés estadounidense en el CSS. CSS, como la mayoría (si no todos) de los demás lenguajes, está escrito en inglés estadounidense, así que mezclar sintaxis como `color:red;` con clases como `.colour-picker{}` carece de consistencia. Previamente he sugerido y recomendado escribir clases bilingües, por ejemplo:

    .color-picker,
    .colour-picker{
    }

En cualquier caso, habiendo trabajado recientemente en un proyecto Sass en el que había docenas de variables de color (p.ej. `$brand-color`, `$highlight-color` etc.), mantener dos versiones de cada variable pronto acabó siendo pesado. También significa el doble de trabajo con cosas como encontrar un reemplazo.

Para favorecer la consistencia, nombra siempre las clases y variables en la versión local del lenguaje con el que estás trabajando.

## Comentarios

Yo uso un estilo de comentarios docBlockesco el cual limito a 80 caracteres de longitud:

    /**
     * Este es un comentario de estilo docBlock
     *
     * Esta es una descripción más larga del comentario, describiendo el código con más
     * detalle Limitamos estas líneas a un máximo de 80 caracteres de longitud.
     *
     * Podemos tener marcado en los comentarios, y se nos anima a hacerlo:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * No prefijamos las líneas de código con un asterisco, ya que hacerlo dificultaría
     * el copiar y pegar.
     */

Deberías documentar y comentar nuestro código tanto como puedas, lo que para tí podría parecer transparente y que se explica por sí mismo, puede no serlo para otro desarrollador. Escribe un trozo de código y luego escribe sobre él.

### Comentarios en esteroides

Hay un número de técnicas más avanzadas que puedes emplear en relación a los comentarios, a saber:

* Selectores cuasi-calificados
* Código de etiquetas
* Indicadores de objeto/extensión

#### Selectores cuasi-calificados

Nunca deberías calificar tus selectores; es decir, nunca deberíamos escribir `ul.nav{}` pudiendo tener simplemente `.nav`. Calificar los selectores disminuye la acción de los selectores, inhibe el potencial de reutilizar una clase en un tipo distinto de elemento y aumenta la especificidad del selector. Todas estas son cosas que deberían ser evitadas a toda costa.

Sin embargo, a veces es útil comunicarle al siguiente desarrollador dónde pretendes que una clase sea utilizada. Tomemos `.product-page` como ejemplo; esta clase suena como si fuera a ser usada en un contenedor de alto nivel, quizás en el elemento `body` o `html`, pero sólo con `.product-page` es imposible adivinarlo.

Cuasi-calificando este selector (p.ej, comentando fuera del selector de tipo guía) podemos comunicar dónde queremos que esta clase sea aplicada, de este modo:

    /*html*/.product-page{}

Ahora podemos ver exactamente dónde aplicar esta clase, pero sin ninguno de los inconvenientes de especificidad o no reusabilidad.

Otros ejemplos podrían ser:

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

Aquí podemos ver dónde pretendemos que cada una de estas clases sean aplicadas sin ni siquiera tener un impacto en la especificidad de los selectores.

#### Etiquetando código

Si escribes un nuevo componente, deja algunas etiquetas relacionadas con su función en un comentario por encima, por ejemplo:

    /**
     * ^navigation ^lists
     */
    .nav{}

    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

Estas etiquetas permiten a los demás desarrolladores encontrar fragmentos de código buscando por función; si un desarrollador necesita trabajar con listas puede buscar `^listas` y encontrar los objetos `.nav` y `.matrix` (y posiblemente más).

#### Indicadores de objeto/extensión

Al trabajar de una forma orientada a objetos, con frecuencia tendrás dos trozos de CSS (siendo una el esqueleto (el objeto) y la otra la piel (la extensión)) muy estrechamente relacionados, pero que se encuentran en lugares muy diferentes. Para establecer una conexión concreta entre el objeto y su extensión, utiliza indicadores de objeto/extensión. Son simplemente comentarios que trabajan así:

En tu hoja de estilos base:

    /**
     * Extendido `.foo` en theme.css
     */
     .foo{}

En tu hoja de estilos plantilla:

    /**
     * Extiende `.foo` en base.css
     */
     .bar{}

Aquí hemos establecido una relación concreta entre dos trozos de código muy separados.

---

## Escribiendo CSS

La sección anterior trataba cómo estructuramos y formamos nuestro CSS; eran reglas muy cuantificables. La siguiente sección es un poco más teórica y trata nuestra actitud y enfoque.

## Construir nuevos componentes

Al construir un componente nuevo escribe una anotación **antes** del CSS. Esto significa que puedes reconocer visualmente qué propiedades CSS se heredan de forma natural y así evitar volver a aplicar estilos redundantes.

Escribiendo anotaciones primero, puedes centrarte en los datos, contenido y semántica y entonces , después, aplicar sólo el CSS y las clases _relevantes_.

## OOCSS

Yo trabajo de una forma OOCSS; divido los componentes en estructura (objetos) y  piel (extensiones). Como **analogía** (nota, no ejemplo) toma lo siguiente:

    .habitación{}

    .habitación--cocina{}
    .habitación--dormitorio{}
    .habitación--baño{}

Tenemos muchos tipos de habitaciones en una casa, pero todas comparten característica similares;   todas ellas tienen suelo, techo, paredes y puertas. Podemos compartir esta información en una clase abstracta `.habitación{}`. Sin embargo, tenemos tipos específicos de habitación que son diferentes a las demás; una cocina seguramente tenga baldosas en el suelo y un dormitorio puede tener alfombras, el cuarto de baño puede no tener una ventana pero el dormitorio probablemente la tenga, cada habitación seguramente tenga la paredes pintadas de diferentes colores. OOCSS nos enseña   a abstraer estilos compartidos en un objeto base y después _extender_ esta información con clases más específicas para añadir los tratos únicos.

Así que, en lugar de construir docenas de componentes únicos, prueba y descubre  patrones de diseños a través de todos ellos, y abstráelos en clases reutilizables; construye estas estructuras como 'objetos' base y después fija las clases a ellos para   extender su estilo para unas circunstancias más únicas.

Si tienes que construir un componente nuevo, divídelo en estructura y piel; construye la  estructura del componente usando clases muy genéricas para que podamos reutilizar esa   construcción y luego usar clases más específicas para cubrirla y añadir tratamientos de diseño.

## Layout

Todos los componente que construyas deberían quedar totalmente libres de anchuras; deberían   permanecer siempre fluidos y sus anchuras deberían estar regidas por un sistema padre/cuadrícula.

Las alturas **nunca** deberían ser aplicadas a los elementos.  Las alturas sólo deberían ser  aplicadas a cosas que tenían dimensiones _antes_ de entrar al sitio (p.ej, imágenes y sprites).  Nunca jamás establezcas alturas en `p`s, `ul`s, `div`s, lo que sea. A menudo puedes conseguir el efecto deseado con interlineado, que es mucho más flexible.

Heights should **never** be be applied to elements. Heights should only be
applied to things which had dimensions _before_ they entered the site (i.e.
images and sprites). Never ever set heights on `p`s, `ul`s, `div`s, anything.
You can often achieve the desired effect with `line-height` which is far more
flexible.

Los sistemas de retícula deberían ser considerados como estantes.   Contienen contenidos pero no son   contenidos en sí.   Tú colocas tus estantes, y después los llenas con tus cosas.   Colocando las celdas de forma separada a los componentes, puedes mover los componentes   con mucha más facilidad que si tuvieran dimensiones aplicadas; esto hace   nuestros front-ends mucho más adaptables y agiliza el trabajar con ellos.

Nunca deberías aplicar estilos a un elemento de una tabla, son únicamente para propósitos de   maquetación.   Aplica el estilo al contenido del _interior_ del elemento de la tabla.   Nunca, bajo _ninguna_ circunstancia, apliques propiedades de caja a un elemento de una tabla.

## Sizing UIs

I use a combination of methods for sizing UIs. Percentages, pixels, ems, rems
and nothing at all.

Grid systems should, ideally, be set in percentages. Because I use grid systems
to govern widths of columns and pages, I can leave components totally free of
any dimensions (as discussed above).

Font sizes I set in rems with a pixel fallback. This gives the accessibility
benefits of ems with the confidence of pixels. Here is a handy Sass mixin to
work out a rem and pixel fallback for you (assuming you set your base font
size in a variable somewhere):

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

I only use pixels for items whose dimensions were defined before the came into
the site. This includes things like images and sprites whose dimensions are
inherently set absolutely in pixels.

### Font sizing

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Shorthand

**Shorthand CSS needs to be used with caution.**

It might be tempting to use declarations like `background:red;` but in doing so
what you are actually saying is ‘I want no image to scroll, aligned top-left,
repeating X and Y, and a background colour of red’. Nine times out of ten this
won’t cause any issues but that one time it does is annoying enough to warrant
not using such shorthand. Instead use `background-color:red;`.

Similarly, declarations like `margin:0;` are nice and short, but
**be explicit**. If you actually only really want to affect the margin on
the bottom of an element then it is more appropriate to use `margin-bottom:0;`.

Be explicit in which properties you set and take care to not inadvertently unset
others with shorthand. E.g. if you only want to remove the bottom margin on an
element then there is no sense in setting all margins to zero with `margin:0;`.

Shorthand is good, but easily misused.

## IDs

A quick note on IDs in CSS before we dive into selectors in general.

**NEVER use IDs in CSS.**

They can be used in your markup for JS and fragment identifiers but use only
classes for styling. You don’t want to see a single ID in any stylesheets!

Classes come with the benefit of being reusable (even if we don’t want to, we
can) and they have a nice, low specificity. Specificity is one of the quickest
ways to run into difficulties in projects and keeping it low at all times is
imperative. An ID is **255** times more specific than a class, so never ever use
them in CSS _ever_.

## Selectors

Keep selectors short, efficient and portable.

Heavily location-based selectors are bad for a number of reasons. For example,
take `.sidebar h3 span{}`. This selector is too location-based and thus we
cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain
styling.

Selectors which are too long also introduce performance issues; the more checks
in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has
four), the more work the browser has to do.

Make sure styles aren’t dependent on location where possible, and make sure
selectors are nice and short.

Selectors as a whole should be kept short (e.g. one class deep) but the class
names themselves should be as long as they need to be. A class of `.user-avatar`
is far nicer than `.usr-avt`.

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

### Over-qualified selectors

As discussed above, qualified selectors are bad news.

An over-qualified selector is one like `div.promo`. You could probably get the
same effect from just using `.promo`. Of course sometimes you will _want_ to
qualify a class with an element (e.g. if you have a generic `.error` class that
needs to look different when applied to different elements (e.g.
`.error{ color:red; }` `div.error{ padding:14px; }`)), but generally avoid it
where possible.

Another example of an over-qualified selector might be `ul.nav li a{}`. As
above, we can instantly drop the `ul` and because we know `.nav` is a list, we
therefore know that any `a` _must_ be in an `li`, so we can get `ul.nav li a{}`
down to just `.nav a{}`.

### Selector performance

Whilst it is true that browsers will only ever keep getting faster at rendering
CSS, efficiency is something you could do to keep an eye on. Short, unnested
selectors, not using the universal (`*{}`) selector as the key selector, and
avoiding more complex CSS3 selectors should help circumvent these problems.

## CSS selector intent

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul{}`…

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul{}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

For a full write up please see my article
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will
**always** want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

## Magic numbers and absolutes

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

## Conditional stylesheets

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

## Debugging

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

## Preprocessors

Sass is my preprocessor of choice. **Use it wisely.** Use Sass to make your CSS
more powerful but avoid nesting like the plague! Nest only when it would
actually be necessary in vanilla CSS, e.g.

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Would be wholly unnecessary in normal CSS, so the following would be **bad**
Sass:

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

If you were to Sass this up you’d write it as:

    .header{}
    .site-nav{
        li{}
        a{}
    }
