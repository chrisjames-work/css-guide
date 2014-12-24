# CSS Guidelines

The following guidelines detail the common standards for writing CSS at Work & Co. They should be seen as best practices, but may be interpreted according to project specific needs. Older projects may also vary in their CSS practices. 

This guide aims to:
- Set standards for code quality across all our projects
- Promote consistency across projects
- Give developers a feeling of familiarity across projects
- Speed up developer onboarding

Whilst this is certainly an opinionated guide, where possible it adheres to industry best practices. It is meant to be a living document, and contributions are welcome. If you disagree with any of the notions put forth here, please [open an issue](https://guides.github.com/features/issues/) and let's talk about it!

This guide owes a gratitude of thanks to the following guides, from which it has borrowed unceremoniously: [Medium](https://gist.github.com/fat/b27700946c777adacdf4), [Google](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml), [Gravity Dept](http://manuals.gravitydept.com/code/css/style-guide), [Lonely Planet](http://ianfeather.co.uk/css-at-lonely-planet/), [Ben Frain](http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/), [CSS Guidelines](http://cssguidelin.es)

TODO:
http://dev.ghost.org/css-at-ghost/
http://markdotto.com/2014/07/23/githubs-css/
https://gist.github.com/fat/a47b882eb5f84293c4ed

## Goals for CSS

**Maintainability**

The foremost goal when writing CSS for an enduring and rapidly changing web application should be long-term maintainability. This means sometimes sacrificing CSS size optimizations for the sake of readability and to make future editing a more manageable process. 

A requisite to this approach is modularity. To quote [Ben Frain](http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/), "a larger total CSS codebase, made up of components that are in many respects insulated from one another, is preferential to a smaller CSS codebase made up of inter-dependent and intrinsically related styles."

**Consistency**

## Formatting and Whitespace

- One space after selector
- Opening curly-brace followed by new line
- One "property:value" pair per line
- Single space after colon
- Semi-colon after all pairs
- Closing curly-brace on new line
- Blank line between rules

**Right:**
```css
.banner {
  left: 0;
  position: fixed;
  top: 0;
}
```

## Preprocessor

On most projects, Work & Co uses [SASS](http://sass-lang.com/) and [Compass](http://compass-style.org/) for preprocessing its CSS.

## IDs vs. Classes

You should ~~almost~~ never need to use IDs for styling purposes. 

## Naming Conventions

There is no ‘right’ naming convention. A good rule to follow though, is to name with intent. Classes such as ```.user-profile``` are both semantically correct, yet not bound contextually and therefore able to be used anywhere.

Basic class names are lowercase with words separated by a dash:

**Right:**
```css
.user-profile {}
.post-header {}
```

**Wrong:**
```css
.userProfile {}
.postheader {}
.top_navigation {}
```

## Element Qualification

Append an element suffix to the class name to clarify the markup pattern (not by qualifying the element type). This keeps specificity low, and improves selector performance.

**Right:**
```css
.articles-list {}
```

**Wrong:**
```css
ul.articles {}
```

Lots more on CSS selectors, from Harry Roberts, [here](http://cssguidelin.es/#css-selectors).

### Element suffixes

Always use suffixes with these elements:

```css
.some-form {}
.some-list {}
.some-table {}
```

## Naming conventions in HTML

A further advantage to following strict clas naming conventions is their positive impact on the markup. Harry Roberts writes in detail about this, [here](http://cssguidelin.es/#naming-conventions-in-html).

## Color Units

All colors should be declared as variables in _variables.scss or the equivalent file.

As developers, we should be cognizant of when the number of color variables on a project gets out of control. Speak to the designer in such cases.

When adding a color variable to _variables.scss:
- use short HEX value if available
- HEX values should be capitalized
- RGB, RGBA and HSLA color units are also valid
- Named colors should be avoided

**Right:**
```css
$white:             #FFF;
$red:               #FE392F;
$black-opacity-30:  rgba(0, 0, 0, 0.3);
```

**Wrong:**
```css
$white:             #fff;
$orange:            #FF9900;
$green;             green;
```

## z-index scale

A z-index scale is encouraged. See [Medium's z-index scale](https://gist.github.com/fat/1f6da6b3bd0311a1f8a0)

## Modules

Always look to abstract modules. Modules should be easily reusable and make sense semantically in different contexts across the whole site.

A name like `.homepage-nav` limits its use. Instead think about writing styles in such a way that they can be reused in other parts of the app. Instead of `.homepage-nav`, try instead `.nav` or `.nav-bar`. Ask yourself if this module could be reused in another context (chances are it could!).

Modules should belong to their own scss file. For example, all general button definitions should belong in _buttons.scss.

To read more about modules in CSS, Jonathan Snook's [SMACSS](https://smacss.com/) is essential.

## Submodules

Use [BEM](https://bem.info/) syntax to define submodules, in a way that relates them directly to their parent module.

```html
<div class="module">
    <h2 class="module__title">Igor</h2>
    <p class="module__desc">Likes ice cream</p>
</div>
```

```css
.module {}
.module__title {}
.module__desc {}
```

## Context Modifiers

Often, modules will vary slightly depending upon their context within the site. The BEM modifier syntax should be used to denote different use case of each module. Examples of modifiers might be the module size (`.btn--sm` or `.btn-lg`), or even page level context (`.nav--home` or `.nav--about`).

```html
<nav class="nav nav--home"></nav>
```

```css
.nav {
  background: $black;
  color: $white;
}

.nav--home {
  background: $red;
}

.nav--about {
  background: $white;
  color: $black;
}
```

## Nesting

Nesting should be kept to a minimum.

If in doubt, create a new module or submodule. Nesting increases specificity which can lead to issues and ultimately hacks (here's looking at you `!important`).

Where nesting does make sense is for pseudo elements, media query mixins and, occasionally, context specific modification (e.g. html/body classes).

**Right:**
```css
.list {
  display: flex;
  
  @include at-large {
    width: 50%;
  }
  
  &:after {
    content: '';
  
    @include at-large {
      width: 50%;
    }  
  }
  
  .noflexbox & {
    display: block;
  }
}
```

**Wrong:**
```css
.list-btn {
  .list-btn-inner {
    .a {
      background: red;
    }
    .a:hover {
      .opacity(0.4);
    }
  }
}
```

## Spacing

CSS rules should be comma seperated but live on new lines:

**Right:**
```css
.content,
.content-edit {
  /* properties */
}
```

**Wrong:**
```css
.content, .content-edit {
  /* properties */
}
```

CSS blocks should be seperated by a single line. not two. not 0. Ditto with nested rules.

**Right:**
```css
.content {
  /* properties */
  
  @include at-medium {
    /* properties */
  }
}

.content-edit {
  /* properties */
}
```

**Wrong:**
```css
.content {
  /* properties */
}
.content-edit {
  /* properties */
}
```

## Utility classes

Utility classes are single responsibility classes. They can help to add consistency and increase DRY-ness. They should have a very narrow scope and may end up being used frequently, due to their separation from the semantics of the document and the theming of a component. 

Utilities may make use of ```!important``` to ensure that their styles always apply ahead of those defined in a component's dedicated CSS.

Utility classes should be prefixed with ```u-```. They should be included in a ```_utilities.scss``` file which would typically be imported at the end of the ```main.scss``` file.

One caveat is that utility classes must only be used where the author is sure that the values set will always be true. For example, consider the application of the styles across different breakpoints and screen sizes.

**Example:**
```css
.u-link-underline {
  text-decoration: underline !important;
  
  .no-touch &:hover {
    text-decoration: none !important;
  }
}
```

```html
<a href="#" class="u-link-underline">This link is underlined</a>
```

## JS Classes

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

Typically, these are classes that are prepended with ```js-```.

**Right:**
```html
<a href="#" class="modal-close js-modal-close">Close</a>
```

```css
.modal-close {
  /* properties */
}
```

```js
$('.js-modal-close').on('click', closeModal);
```

## Comments

Comments are good. Indeed, writing lots of meaningful comments is strongly encouraged. Use comments as liberally as needed in your scss files. They will be stripped out on compilation of the CSS file.

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that ```color: red;``` will make something red, but if you’re using ```overflow: hidden;``` to clear floats—as opposed to clipping an element’s overflow—this is probably something worth documenting.

At the very minimum, use comments to seperate logical groups of styles within a document. For this, always use the following style comment:


**Right:**
```css
/* ==========================================================================
   Title of page
   ========================================================================== */

/* Title of section
   ========================================================================== */
```

**Wrong:**
```css
/* Title of section
 **********************/
```

## Quotes

Quotes are optional in CSS. Work & Co chooses to use quotes as it is visually clearer that the string is not a selector or a style property. 

Single quotes are the way forward.

**Right:**
```css
background-image: url('/img/you.jpg');
font-family: 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial;
```

**Wrong:**
```css
background-image: url("/img/you.jpg");
font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
```

## Structure

### Parameter Order

CSS parameters should be alphabetized. 

**Right:**
```css
.sticky {
  left: 0;
  position: fixed;
  top: 0;
  width: 100%;
}
```

**Wrong:**
```css
.sticky {
  display: block;
  color: red;
  text-decoration: none;
  font-size: 10rem;
}
```

When using mixins, consider the CSS output and order accordingly. 

**Right:**
```css
.article {
  @include opentype-regular;
  @include font-size(1.5);
  line-height: 20px;
}
```

If using vendor-prefixed rules, group the related rules - but preferably use [autoprefixer](https://github.com/postcss/autoprefixer) or a Compass mixin instead.

**Right:**
```css
.article {
  background: none;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  display: block;
}
```

**Better (with autoprefixer):**
```css
.article {
  background: none;
  box-sizing: border-box;
  display: block;
}
```

**Better (with Compass mixin):**
```css
.article {
  background: none;
  @include box-sizing(border-box);
  display: block;
}
```

## Folder structure

Modular CSS lends itself to writing a new file for each CSS component. The CSS ```@import``` function makes this easy. As well as component modules, each project should include a number of base modules and global project helper files. The typical basic project styles directory structure will look like this:

```css
/styles
  /base
    _grid.scss
    _scaffolding.scss
    _typography.scss
  /modules
    _footer.scss
    _header.scss
    _nav.scss
  _mixins.scss
  _normalize.scss
  _utils.scss
  _variables.scss
  main.scss
```

And the ```main.scss``` file should look like this:

```
// Compass framework imports
@import 'compass/css3';

// Core variables and mixins
@import 'variables';
@import 'mixins';

// Reset
@import 'normalize';

// Base styles
@import 'base/typography';
@import 'base/scaffolding';
@import 'base/grid';

// Modules
@import 'modules/header';
@import 'modules/nav';
@import 'modules/footer';

// Utilities
@import 'utils
```

## SASS/Compass

### Mixins

Make sure to take into consideration the output of using SASS and Compass mixins. They are best used for grouping browser-specific code or as powerful ways to contain functionality. They are generally not a good way to add additional styles to an element as you will be sending duplicate styles across the wire.

### Extends

Should not necessarily be avoided completely, but must be used with caution and with an eye on the outputted CSS. In the wrong hands, the SASS @extend function can cause serious selector bloat and headaches for your fellow developers. More [here](http://www.sitepoint.com/avoid-sass-extend/) and [here](http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/#l22).

### Vendor-Prefixes

Compass provides [mixins](http://compass-style.org/reference/compass/css3/) for style declartions that require vendor prefixes. It is generally good practice to make use of these wherever possible.

```css
@include border-radius(10px);
```

A possible alternative is to use [autoprefixer](https://github.com/postcss/autoprefixer). Autoprefixer is a build tool which adds vendor prefixes to CSS rules using values from [caniuse](http://caniuse.com/). Autoprefixer may result in more concise CSS output than Compass mixins, depending on the level of required browser support.

## Refactoring

Get rid of as much code as possible when refactoring. Don't be precious about keeping things around in case it might be needed. That's why we use version control!

To quote [Ben Frain](http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/), "more CSS bloat accumulates due to author deletion fear (“I don’t know what that’s for so just leave it in”) than duplicated styles".

## Misc

* Keeping CSS DRY is important, but not if it sacrifices maintainability.
* Use [normalize.css](https://github.com/necolas/normalize.css/)
* [Grunticon](https://github.com/filamentgroup/grunticon) has proved a very useful tool for serving svg icons and logos.
