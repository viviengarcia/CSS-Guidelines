# Guide des feuilles de styles CSS, conseils et bonnes pratiques

---

## Traductions

* [Russian/русский](https://github.com/matmuchrapna/CSS-Guidelines/blob/master/README%20Russian.md)
* [French/Français](https://github.com/flexbox)

---

En travaillant sur de grands projets d'envergure fonctionnant avec des dizaines de développeurs, il est
important que nous travaillions tous de façon unifiée avec pour objectif de :

* Garder les feuilles de style maintenables
* Garder le code transparent et lisible
* Garder les feuilles de style extensibles

Il existe plusieures techniques que nous devons employer pour satisfaire ces objectifs.

La première partie de ce document traitera de la syntaxe, du formatage et de la taxonomie CSS
La deuxième partie traitera de l'approche, l'état d'esprit et l'attitude à avoir pour écrire et architecturer du CSS.
Passionnant, hein?

## Contenu

* [Anatomie d'un document CSS](#css-document-anatomy)
  * [Généralités](#general)
  * [Un seul fichier ou plusieurs fichiers ?](#one-file-vs-many-files)
  * [Table des matières](#table-of-contents)
  * [Titre des sections](#section-titles)
* [Ordre des sources](#source-order)
* [Anatomie d'une règle](#anatomy-of-rulesets)
* [Classes en HTML](#html-class)
* [Convention de nomage](#naming-conventions)
  * [Ancres javascript](#js-hooks)
  * [Localisation](#internationalisation)
* [Commentaires](#comments)
  * [Commentaires sous stéroïdes](#comments-on-steroids)
    * [Sélecteurs spécifiques](#quasi-qualified-selectors)
    * [Code des balises](#tagging-code)
    * [Association des objets](#objectextension-pointers)
* [Ecrire du CSS](#writing-css)
* [Construire de nouveau composants](#building-new-components)
* [CSSOO](#oocss)
* [Mise en page](#layout)
* [Taille des interfaces](#sizing-uis)
  * [Taille des polices](#font-sizing)
* [Sténographie](#shorthand)
* [IDs](#ids)
* [Sélecteurs](#selectors)
  * [Qualification des Sélecteurs](#over-qualified-selectors)
  * [Performance des sélecteurs](#selector-performance)
* [CSS selector intent](#css-selector-intent)
* [`!important`](#important)
* [Nombres magiques et absolu](#magic-numbers-and-absolutes)
* [Styles conditionnel](#conditional-stylesheets)
* [Débugage](#debugging)
* [Préprocesseurs](#preprocessors)

---

## Anatomie d'un document CSS

Peu importe le document, il faut toujours essayer de garder un formatage commun. Cela signifie une cohérence des commentaires, de la syntaxe et des règles de nommage.

### Généralités

Limitez vos feuilles de style avec un maximum de 80 caractères de longeur lorsque cela est possible.
Des exceptions peuvent être la syntaxe des dégradés ainsi que les URL dans les commentaires. Il n'y a rien que nous puissions faire à ce sujet.

Concernant l'indentation je préfère 2 espaces au lieu des tabulations et écrire sur plusieures lignes CSS.

### Un seul fichier ou plusieurs fichiers ?

Certaines personnes préfèrent travailler avec de simple fichiers volumineux.
Cela fonctionne très bien pour les petits projets mais avec les directives suivantes
vous aller très vite renconter des problèmes.
Depuis l'arrivée de sass j'ai commencé à séparer mes feuilles de styles en petits fichiers.
Cette méthode est aussi très bonne. Quelle que soit la méthode que vous choisissez, les règles suivantes et
les lignes directrices s'appliquent. La seule différence notable est en ce qui concerne notre table des
matières et nos titres de section. Lisez la suite pour plus d'explications ...

### Table des matières

Au sommet des feuilles de style, je maintiens une table des matières qui détaillera les
sections contenues dans le document, par exemple:


    /*------------------------------------*\
        $CONTENU
    \*------------------------------------*/
    /**
     * CONTENU.............Vous êtes en train de le lire
     * RESET...............Réinitialisation des styles par défaut
     * FONT................Les polices de caractère
     */

Ceci va permettre au prochain développeur de savoir exactement ce qu'il va trouver dans
ce fichier. Chaque élément de la table des matières correspond directement à un titre de section.

Si vous travaillez sur une grande feuille de style, la section correspondante sera également
dans ce fichier. Si vous travaillez sur plusieurs fichiers, chaque élément de la
table des matières correspondra à une inclusion.

### Titres de section

La table des matières ne serait d'aucune utilité si elle ne possède pas une section correspondante.
Les sections sont désignées ainsi :

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

Le préfixe `$` dans une section permet de faire une recherche rapide ([Cmd|Ctrl]+F ou ([Ctrl|Shift]+F))

Si vous travaillez dans une grande feuille de style, laissez cinq (5) retours chariot
entre chaque section, comme ceci :

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Vos
    styles
    par défaut]





    /*------------------------------------*\
        $FONT
    \*------------------------------------*/

Ce gros morceau d'espaces est rapidement perceptible lorsque vous faites défiler
des fichiers plus volumineux.

Si vous travaillez sur plusieurs feuilles de styles, démarrez chacun de ces
fichiers avec un titre de section. Il n'est pas nécessaire d'inclure des retours chariot.

## Ordre des sources

Essayez d'écrire des feuilles de style dans leur ordre de spécificité. Cela garantit que vous
conservez l'avantage de l'héritage en compte ainsi que le <i>C</i> du CSS qui signifie cascade.

Une feuille de style bien ordonnée ressemblera à ceci :

1. **Reset** – réinitialisation des propriétés.
2. **Elements** – propriétés de bases `h1`, `ul` etc.
3. **Objets et abstractions** — patrons de conception génériques.
4. **Composants** – composants complets construits à partir d'objets et de leurs extensions.
5. **Atouts de style** – états des erreurs etc.

Cela signifie que, lorsque vous descendez dans le document chaque section s'appuie et
hérite de la précédente. Il devrait y avoir moins d'annulation de
styles, moins de problèmes de spécificité, moins de création de style tous azimuts et une
meilleure architecture de vos feuilles.

Pour en savoir plus, Je vous recommancde chaudement de lire
[SMACSS](http://smacss.com) de Jonathan Snook.

## Anatomie d'une règle

    [selecteur]{
        [propriété]:[valeur];
        [<- Déclaration ->]
    }

J'applique un certain nombre de normes concernant la structure des règles CSS.

* Utiliser `-` pour délimiter les noms de classe (sauf pour cet exemple de,
  [convention de nommage](#naming-conventions))
* 2 espaces d'indentation
* Multi-ligne
* Déclaration par ordre de pertinence (et non alphabétique)
* Indenter les déclarations de préfixes et aligner leurs valeurs
* Indenter les règles en reflètant le DOM
* Toujours inclure le point-virgule final

Un petit exemple :

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

Ici, nous pouvons voir que `.widget-heading` doit être un enfant de `.widget` que nous avons
indenté .widget-heading` d'un niveau plus profond que `.widget`. C'est une
information utile pour les développeurs qui peuvent être scannées seulement d'un coup d'œil
suivant l'indentation de notre ensemble de règles.

Nous pouvons également voir que les déclarations de `.widget-heading` sont triés par leur
pertinence. `.widget-heading` doit être un élément textuel si nous commençons avec nos
règles de texte, suivie par toutes les autres.

Une exception à notre règle multi-ligne pourrait être dans le cas suivant :

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

Dans cet exemple (extrait du système de grille d'[inuit.css](
https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88))
il est plus logique de tout concentrer sur une seule ligne.

## Convention de nommage

La plupart du temps, j'utilise simplement des classes délimités par des traits d'union (ex. `. Foo-bar`, pas
`. foo_bar` ou `. FooBar), mais dans certaines circonstances, j'utilise la notation BEM (Block,
Élément, Modifieur).

<abbr title="Block, Element, Modifier">BEM</abbr> est une méthode pour nommer
et classifier vos sélecteurs CSS de façon à les rendre beaucoup plus strict,
transparent et informatif.

La convention de nommage suit ce modèle :

    .block{}
    .block__element{}
    .block--modifieur{}

* `.block` représente le niveau supérieur d'une abstraction ou d'un composant.
* `.block__element` représente un descendant de `.bloc` puisqu'il contribue à former `.bloc`
   dans son ensemble.
* `.block--modifieur` représente un état ou une version différente de `.block`.

Une **analogie** du fonctionement de la méthode BEM :

    .personne{}
    .personne--femme{}
        .personne__main{}
        .personne__main--gauche{}
        .personne__main--droite{}

Ici, nous pouvons voir que l'objet de base que nous décrivons est une personne, et qu'une
autre type de personne pourrait être une femme. Nous pouvons également voir que les gens ont
des mains, ce sont des sous-parties des personnes, et il y a différentes variantes,
comme main gauche et main droite.

Nous pouvons maintenant nommer nos sélectionneurs en fonction de leurs objets de base et nous
pouvons également savoir ce que le sélecteur fait, est-il un sous-composant (`__`) ou une
variation (`--`)?

Ainsi, `.page-wrapper` est un sélecteur autonome, il ne fait pas partie d'une
abstraction ou d'un composant et comme tel il nommé correctement.
Toutefois, `.widget-heading`
 _est_ lié à un composant, c'est un enfant du constructeur `.widget`.
Nous devrions renommer cette classe `.widget__heading`.

La notation BEM est un peu laide, plus verbeuse, mais elle nous donne beaucoup plus de pouvoir dans le glanage d'informations sur les fonctions et sur les relations entre les éléments, uniquement avec leurs nom de classes. En outre, la syntaxe BEM sera généralement très bien compressée (gzip) puisqu'elle favorise / fonctionne bien avec la répétition.

Peu importe si vous avez besoin d'utiliser BEM ou non, assurez-vous de toujours nommer vos classes judicieusement; garder un nom aussi court que possible, mais aussi long que nécessaire. S'assurer que les objets ou les abstractions sont très vaguement nommées (par exemple `.ui-list`, `.media`) pour permettre une meilleure réutilisation.
Les extensions des objets devraient être nommées beaucoup plus explicitement
(par exemple `.user-avatar-link`). Ne vous inquiétez pas du montant ou de la taille de
vos classes; gzip compresse le code bien écrit _incroyablement_ bien.

### Classes en HTML

Dans le but de rendre les choses plus faciles à lire, séparez les classes dans
votre HTML avec deux (2) espaces :

    <div class="foo--bar  bar__baz">

L'augmentation des espaces blancs devrait, faciliter le repérage et la lecture
de plusieurs classes.

### Ancres Javascript

**N'utilisez jamais une classe de _style_ CSS pour vos ancres Javascript**
Associer un comportement javascript à une classe de style  signifie que nous ne
pourrons jamais avoir l'un sans l'autre.

Si vous avez besoin de se lier un évènement à une balise, créez une classe JS dans votre CSS.
Il s'agit simplement d'une classe avec un nommage `.js-`, par exemple `.js-toggle`, `.js-drag-and-drop`.
Cela signifie que nous pouvons joindre les deux classes JS et CSS pour notre balisage, sans avoir
de chevauchements gênants.

    <th class="is-sortable  js-is-sortable">
    </th>

Le balisage ci-dessus contient deux classes; la première est associée au style du
tableau, la deuxième à une fonction de tri.

### Localisation

Despite being a British developer—and spending all my life writing <i>colour</i>
instead of <i>color</i>—I feel that, for the sake of consistency, it is better
to always use US-English in CSS. CSS, as with most (if not all) other languages,
is written in US-English, so to mix syntax like `color:red;` with classes like
`.colour-picker{}` lacks consistency. I have previously suggested and advocated
writing bilingual classes, par exemple :

En dépit d'être un développeur Anglais, je passe ma vie à écrire <i>colour</i>
au lieu de <i>color</i> Je pense que, dans un souci de cohérence, il est préférable
d'utiliser toujours un anglais Américain en CSS. CSS, comme avec la plupart (sinon tous) les autres langages,
est écrit en anglais-US. Mélanger une syntaxe `color: red;` avec des classes comme
`.colour-picker{}` peut manquer de cohérence. J'ai déjà proposé et défendu une écriture des classes bilingues,

    .color-picker,
    .colour-picker{
    }

Cependant, après avoir récemment travaillé sur un projet de grande envergure en Sass où il y avait
des dizaines de variables de couleur (par exemple `$brand-color`, `$highlight-color` etc.),
maintenir deux versions de chaque variable est vite devenu ennuyeux. Cela signifie également
deux fois plus de travail avec des choses comme "rechercher et remplacer".

Dans un souci de cohérence, nommez toujours vos classes et vos variables dans les paramètres régionaux
de la langue que vous utilisez.

Vous pouvez aussi vous mettre d'accord avec votre équipe de développement pour n'utiliser que de l'anglais pour vos projets. Cela vous permettra d'améliorer votre maîtrise de la langue de Shakespeare.

## Commentaires

J'utilise un style de commentaire docBlock où la taille est limitée à 80 caractères :

    /**
     * Ceci est un commentaire de style DocBlock
     *
     * Ceci est une description plus détaillée
     * Nous limitons ces lignes pour un maximum de 80 caractères.
     *
     * Nous pouvons avoir des balises dans les commentaires :
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * Nous n'avons pas de préfixe avec une étoile pour faciliter le
     * copier-coller.
     */

Vous devez documenter et commenter notre code autant que vous le pouvez, quoi qu'il arrive.
Cela peut sembler trop transparent ou explicite pour vous mais peut-être pas pour un autre dev.
Chaque nouveau morceau de code doit-être documenté.

### Commentaires sous stéroïdes

Il ya un certain nombre de techniques plus avancées que vous pouvez employer en ce qui concerne
les commentaires, à savoir :

* Les sélecteurs spécifiques
* Le codage des balises
* l'association des objets

#### Sélecteurs spécifiques

Vous ne devriez jamais qualifiez vos sélecteurs, c'est-à-dire que nous ne devrions jamais écrire
`ul.nav{}` si vous pouvez juste avoir `.nav`. Qulaifier vos sélecteurs diminue leur
rendement, inhibe le potentiel de réutilisation d'un objet sur un autre type d'
élément et augmente la spécificité du sélecteur. Ce sont toutes des choses qui
doivent être évitée à tout prix.

Cependant, il est parfois utile de communiquer à d'autre(s) développeur(s) où
vous avez l'intention d'utiliser une classe. Prenons `.product-page` pour exemple; cette
classe sonne comme si elle serait utilisée sur un conteneur de haut niveau, peut-être avec
`html` ou `body`, mais seulement avec `.product-page` il est impossible de le
savoir.

Par quasi-qualification de ce sélecteur (autrement dit en commentant le sélecteur de premier plan),
nous pouvons communiquer nos intentions pour cette classe :

    /*html*/.product-page{}

We can now see exactly where to apply this class but with none of the
specificity or non-reusability drawbacks.

Nous pouvons maintenant voir exactement où s'applique cette classe, sans avoir les inconvénients de la
la spécificité ou de la non-réutilisation.

D'autres exemples pourraient être: :

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

Dans ces cas, nous savons le contexte d'utilisation de ces classes sans jamais impacter la spécificité des selecteurs

#### Code des balises

Si vous écrivez un nouveau composant, laissez certaines balises relatives à son utilisation dans
un commentaire ci-dessus, par exemple:

    /**
     * ^navigation ^lists
     */
    .nav{}

    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

Ces balises permettent à d'autres développeurs de trouver des bouts de code en recherchant
une fonction, si un développeur a besoin pour travailler avec des listes ils peuvent rechercher
`^lists` et trouver les objets associés `.nav` et `.matrix` (et probablement plus).

#### Association des objets

When working in an object oriented manner you will often have two chunks of CSS
(one being the skeleton (the object) and the other being the skin (the
extension)) that are very closely related, but that live in very different
places. In order to establish a concrete link between the object and its
extension with use <i>object/extension pointers</i>. These are simply comments
which work thus:

Lorsque l'on travaille d'une manière orientée objet, vous aurez souvent deux morceaux de CSS
(l'une étant le squelette (l'objet) et l'autre étant la peau (l'
extension)) qui sont très étroitement liés, mais qui vivent dans des endroits très différent.
Afin d'établir un lien béton entre l'objet et son extension de l'utilisation objet / extension pointeurs <i> </ i>. Ce sont simplement des commentaires qui oeuvrent ainsi:

In your base stylesheet:

    /**
     * Extend `.foo` in theme.css
     */
     .foo{}

In your theme stylesheet:

    /**
     * Extends `.foo` in base.css
     */
     .bar{}

Here we have established a concrete relationship between two very separate
pieces of code.

---

## Ecrire du CSS

The previous section dealt with how we structure and form our CSS; they were
very quantifiable rules. The next section is a little more theoretical and deals
with our attitude and approach.

## Construire de nouveau composants

When building a new component write markup **before** CSS. This means you can
visually see which CSS properties are naturally inherited and thus avoid
reapplying redundant styles.

By writing markup first you can focus on data, content and semantics and then
apply only the relevant classes and CSS _afterwards_.

## CSSOO

I work in an OOCSS manner; I split components into structure (objects) and
skin (extensions). As an **analogy** (note, not example) take the following:

    .room{}

    .room--kitchen{}
    .room--bedroom{}
    .room--bathroom{}

We have several types of room in a house, but they all share similar traits;
they all have floors, ceilings, walls and doors. We can share this information
in an abstracted `.room{}` class. However we have specific types of room that
are different from the others; a kitchen might have a tiled floor and a bedroom
might have carpets, a bathroom might not have a window but a bedroom most likely
will, each room likely has different coloured walls. OOCSS teaches us to
abstract the shared styles out into a base object and then _extend_ this
information with more specific classes to add the unique treatment(s).

So, instead of building dozens of unique components, try and spot repeated
design patterns across them all and abstract them out into reusable classes;
build these skeletons as base ‘objects’ and then peg classes onto these to
extend their styling for more unique circumstances.

If you have to build a new component split it into structure and skin; build the
structure of the component using very generic classes so that we can reuse that
construct and then use more specific classes to skin it up and add design
treatments.

## Mise en page

All components you build should be left totally free of widths; they should
always remain fluid and their widths should be governed by a parent/grid system.

Heights should **never** be be applied to elements. Heights should only be
applied to things which had dimensions _before_ they entered the site (i.e.
images and sprites). Never ever set heights on `p`s, `ul`s, `div`s, anything.
You can often achieve the desired effect with `line-height` which is far more
flexible.

Grid systems should be thought of as shelves. They contain content but are not
content in themselves. You put up your shelves then fill them with your stuff.
By setting up our grids separately to our components you can move components
around a lot more easily than if they had dimensions applied to them; this makes
our front-ends a lot more adaptable and quick to work with.

You should never apply any styles to a grid item, they are for layout purposes
only. Apply styling to content _inside_ a grid item. Never, under _any_
circumstances, apply box-model properties to a grid item.

## Taille des interfaces

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

### Taille des polices

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Sténographie

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

## Sélecteurs

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

### Qualification des Sélecteurs

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

### Performance des sélecteurs

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

## Nombres magiques et absolus

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

## Styles conditionnel

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

## Débugage

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

## Préprocesseurs

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
