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
* [Dimensionando ULs](#dimensionando-uls)
  * [Dimensionando fuentes](#Dimensionando-fuentes)
* [Abreviaturas](#abreviaturas)
* [IDs](#ids)
* [Selectores](#selectores)
  * [Selectores sobre-calificados](#selectores-sobre-calificados)
  * [Rendimiento de los selectores](#rendimiento-de-los-selectores)
* [Selector CSS fijo](#selector-fijo)
* [`!important`](#important)
* [Números mágicos y absolutos](#números-mágicos-y-absolutos)
* [Hojas de estilo condicionales](#hojas-de-estilo-condicionales)
* [Depurado](#depurado)
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

## Dimensionando ULs

Yo uso una combinación de métodos para dimensionar UIs.   Porcentajes, píxeles, ems, rems   y nada en absoluto.

Los sistemas de retícula deberían, idealmente, ser colocados en porcentajes.   Ya que yo utilizo sistemas de retícula   para determinar las anchuras de las columnas y páginas, puedo dejar los componentes completamente libres de   cualquier dimensión (como expliqué antes).

Pongo el tamaño de la fuente en rems con un pixel fallback. Esto da los beneficios de accesibilidad de los ems con la seguridad de los píxels.   Aquí  tienes un útil recurso de mixin de Sass para   usar píxeles y rems (suponiendo que establezcas tu tamaño de fuente   base en una variable en algún sitio): 

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

Yo solamente uso los píxeles para los elementos cuyas dimensiones estaban ya definidas antes de entrar al   sitio.   Esto incluye cosas como imágenes y sprites cuyas dimensiones son   intrínsecamente establecidas absolutamente en píxels.

### Dimensionando fuentes

Yo defino una serie de clases parecida a un sistema de retícula para dimensionar las fuentes.   Estas   clases pueden ser utilizadas para dar al texto un estilo de jerarquía de encabezados de doble línea.   Para una   explicación completa de cómo funciona esto, por favor, dirígete a mi artículo [Dimensionado de fuentes práctico y pragmático en CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css).  

## Abreviaturas

**El CSS abreviada debe usarse con precaución.**

Tal vez sea tentador utilizar declaraciones como `background: red;` pero haciendo esto, lo que realmente estás diciendo es "Quiero que ninguna imagen haga scroll, alineado superior izquierdo, repetir X e Y, y un color de fondo rojo".   Nueve veces de cada diez, esto   no provocará ningún problema, pero la vez que sí lo hace es lo bastante molesta como para garantizar   no usar este tipo de anotación.   En su lugar, utiliza `background-color: red;`.

De forma similar, las declaraciones como `margin:0;` son cortas y agradables, pero **sé explícito**.  Si realmente sólo quieres que afecte al margen al final de un elemento, entonces es más apropiado utilizar `margin-bottom:0;`.

Sé explícito con las propiedades que estableces y preocúpate de no descolocar las demás sin darte cuenta   con abreviaciones   P.ej, si sólo quieres quitar el margen inferior de un   elemento, entonces no tiene sentido establecer todos los márgenes a cero con `margin:0;`.

La abreviación es buena, pero fácilmente mal utilizada.

## IDs

Una nota rápida sobre los IDs en CSS antes de que nos metamos en los selectores en general.  

**NUNCA uses IDs en CSS.**

Pueden ser usados en tus anotaciones para JS e identificadores de fragmentos, pero utiliza sólo   las clases para diseñar. ¡No quieres ver ni un solo ID en ninguna hoja de estilos!

Las clases tienen la ventaja de ser reutilizables (incluso si no queremos, podemos) y tienen una especificidad baja y buena. La especificidad es una de las formas más rápidas de encontrarse con dificultades en los proyectos, y mantenerla baja siempre es fundamental. Un ID es **255** veces más específico que una clase, así que no los utilices en CSS _nunca_.

## Selectores

Mantén los selectores cortos, eficientes y transferibles.

Los selectores con una ubicación muy fuerte son malos por varias razones. Por ejemplo, tome `.sidebar h3 span{}`. Este selector está demasiado localizado, y por lo tanto no podemos mover ese `span` fuera de un `h3` fuera de `.sidebar` y mantener el estilo.  

Los selectores que son demasiado largos también presentan problemas de rendimiento; cuantas más comprobaciones en un selector (p. ej, `.sidebar h3 span` tiene tres comprobaciones, `.content ul p a` tiene cuatro), más trabajo tiene que hacer el navegador.

Asegúrate de que los estilos no sean dependientes de la localización donde sea posible, y asegúrate de que los selectores son agradables y cortos.

Los selectores en su totalidad deberían mantenerse cortos (p.ej, una clase de profundidad) pero los nombres de las clases en sí deberían ser tan largos como necesiten ser. Una clase de `.user-avatar` es mucho mejor que `.usr-avt`.

**Recuerda:** las clases no son semánticas o no semánticas; son sensatas o insensatas!  Deja de estresarte por los nombres de clases "semánticos" y elige algo sensato y seguro.

### Selectores sobre-calificados

Como analizamos anteriormente, los selectores calificados son malas noticias.  

Un selector sobre calificado es uno como `div.promo`. Probablemente podrías obtener el  mismo efecto usando solamente `.promo`. Por supuesto que algunas veces _querrás_  calificar una clase con un elemento (p.ej, si tienes una clase `.error` genérica que necesita resultar diferente cuando se aplica a elementos diferentes (p.ej, `.error{ color:red;` } `div.error{padding: 14px; }`)), pero por lo general, evítalo cuando sea posible.  

Otro ejemplo de selector sobre-calificado podría ser `ul.nav li a{}`. Como  anteriormente, podemos omitir instantáneamente el `ul`, y como sabemos que `.nav` es una lista, sabemos que cualquier `a` tiene que estar en un `li` , así que podemos reducir `ul.nav li a{}`   a solo `.nav a{}`.  

Another example of an over-qualified selector might be `ul.nav li a{}`. As

### Rendimiento de los selectores

Aunque es cierto que los navegadores siempre seguirán siendo cada vez más rápidos renderizando CSS, la eficiencia es algo que puedes hacer para estar pendiente. Selectores breves y no anidados, no usar el selector universal (`*{}`) como selector clave, y evitar selectores CSS3 más complejos debería ayudar a evitar estos problemas.

## Selector CSS fijo

En lugar de usar selectores para profundizar el DOM a un elemento, a menudo es mejor poner una clase en el elemento que específicamente quieres dar estilo. Vamos a hacer ejemplo específico con un selector como `.header ul{}`…

Vamos a imaginar que `ul` es en realidad la principal navegación para nuestra sitio web. Vive en el header como quizá esperas y es el unico `ul` ahí actualmente; `.header ul{}` funcionaría, pero no es ideal ni aconsejable. No tiene mucho futuro en pruebas y ciertamente no explícita lo suficiente. Tan pronto como añadamos otro `ul` a éste header adoptará el estilo de nuestra navegación principal y probablemente no sea lo que queramos. Esto significa que también tenemos que volver a rehacer un monton de código o deshacer un montón de estilos en subsecuentes `uls` en este `.header` para eliminar los efectos del selector de largo alcance.  

La intención te tu selector debe coincidir con el motivo por el que le das estilo a algo; pregúntate a ti mismo **“¿estoy seleccionando esto porque es un `ul` dentro de `.header` o porque es la navegación principal de mi sito?"**. La respuesta a esto determinará tu selector.

Asegúrate de que tu selector nunca es un selector elemento/tipo o clases objecto/abstracciones. Realmente tu nunca quieres ver selectores como `.sidebar ul{}` o `.footer .media{}` en nuestras hojas de estilos.

Sé explícito; fija el elemento al que quieres influir, no a sus padres. Nunca asumas que el marcado no va a cambiar.   **Escribe selectores que influyan en lo que tu quieres, no en lo que ocurre allí ya**.

Para una descripción completa por favor vea mi artículo
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

Esta bien usar `!important` únicamente sobre clases "helper" (de ayuda). Añadir `!important` preventivamente está bien, ej: `.error{ color:red !important}`, como sabes, esta regla **siempre** tendrá prioridad.

Usar `!important` reiteradamente, por ejemplo para no verse en situaciones específicamente desagradables no está recomendado. Vuelve a escribir tu CSS e intentan combatir estos problemas refactorizando tus selectores. Mantener tus selectores cortos y evitar añadir ID's te ayudará enormemente  

## Números mágicos y absolutos

Un número mágico es un número que se utiliza porque "simplemente funciona".  Son malos porque rara vez trabajan con alguna razón real y no están por lo general muy preparados para cambios futuros y flexibilidad. Estos números tienden a arreglar síntomas y no problemas.

Por ejemplo, usando `.dropdown-nav li:hover ul{ top:37px; }` para mover un dropdown al fondo de nav al hacer hover es malo, `37px` es un número mágico. `37px` solamente funciona aquí porque en este escenario particular el `.dropdown-nav` pasa para tener `37px` de altura.

En su lugar deberías usar `.dropdown-nav li:hover ul{ top:100%; }` que significa que no importa como de alto sea `.dropdown-nav`, el dropdown estará siempre al 100% desde la parte superior.

Cada vez que escriba un número en el código piensa dos veces; si lo puedes evitar usando palabras clave o alias (ej. `top: 100%` significa 'todos los caminos desde arriba') o &mdash;aún mejor&mdash; no deberías medir nada en absoluto.  

Cada medida no modificable que se establece es un compromiso que podría no querer conservar necesariamente.

## Hojas de estilo condicionales

Las hojas de estilo para IE pueden, en general, ser totalmente evitadas. La única vez que una hoja de estilo para IE   puede ser necesaria es para evitar carencia de soporte (ej. ajuste en imágenes PNG).

Como regla general, todas las reglas de diseño y caja-modelo pueden y _van_ a trabajar sin una hoja de estilo IE si refactoriza y rehace su CSS. Esto significa que nunca querrás ver `<!--[if IE 7]> element{ margin-left:-9px; } < !   [endif]-->` u otro como CSS que esta claramente usando estilo arbitrario para hacer 'que cosas funcionen'.

## Depurado

Si se encuentra con un problema de código CSS **espera antes de empezar a añadir más** en un intento de arreglarlo. El problema existe en CSS que ya está escrito, ¡más CSS no es la respuesta correcta!

Elimina trozos de código y CSS hasta que el problema desaparezca, entonces podrás determinar en qué parte del código radica problema.

Puede ser tentador poner un `overflow:hidden;` para ocultar algún efecto extraño, pero el overflow nunca suele ser el problema; **arregla el problema, no sus síntomas**.

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

