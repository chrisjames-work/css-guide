# CSS Guidelines

The following guidelines detail the common standards for writing CSS at Work & Co. They should be seen as best practices, but may be interpreted according to project specific needs. Older projects may also vary in their CSS practices. 

This guide owes a gratitude of thanks to the following guides, from which it has borrowed unceremoniously: [Medium](https://gist.github.com/fat/b27700946c777adacdf4), [Gravity Dept](http://manuals.gravitydept.com/code/css/style-guide)

## Formatting and Whitespace

- One space after selector
- Opening curly-brace followed by new line
- One "property:value" pair per line
- Single space after colon
- Semi-colon after all pairs
- Closing curly-brace on new line
- Blank line between rules

**Right:**
```sass
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

Basic class names are lowercase with words separated by a dash:

**Right:**
```sass
.user-profile {
  â€¦
}

.post-header {
  â€¦
}
```

**Wrong:**
```sass
.userProfile {
  â€¦
}

.postheader {
  â€¦
}

.top_navigation {
  â€¦
}
```

## Color Units

All colors should be declared as variables in _variables.scss or the equivalent file.

As developers, we should be cognizant of when the number of color variables on a project gets out of control. Speak to the designer in such cases.

When adding a color variable to _variables.scss:
- use short HEX value if available
- HEX values should be capitalized
- RGB, RGBA and HSLA color units are also valid
- Named colors should be avoided

**Right:**
```sass
$white:             #FFF;
$red:               #FE392F;
$black-opacity-30:  rgba(0, 0, 0, 0.3);
```

**Wrong:**
```sass
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

```sass
.module {
  â€¦
}

.module__title {
  â€¦
}

.module__desc {
  â€¦
}
```

## Context Modifiers

Often, modules will vary slightly depending upon their context within the site. The BEM modifier syntax should be used to denote different use case of each module. Examples of modifiers might be the module size (`.btn--sm` or `.btn-lg`), or even page level context (`nav--home` or `.nav--about`).

```html
<nav class="nav nav--home"></nav>
```

```sass
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
```sass
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
```sass
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
```sass
.content,
.content-edit {
  â€¦
}
```

**Wrong:**
```sass
.content, .content-edit {
  â€¦
}
```

CSS blocks should be seperated by a single line. not two. not 0. Ditto with nested rules.

**Right:**
```sass
.content {
  â€¦
  
  @include at-medium {
    â€¦
  }
}

.content-edit {
  â€¦
}
```

**Wrong:**
```sass
.content {
  â€¦
}
.content-edit {
  â€¦
}
```

## Comments

Use comments as liberally as needed in your scss files. They will be stripped out on compilation of the CSS file.

At the minimum, use comments to seperate logical groups of styles within a document. For this, always use the following style comment:


**Right:**
```sass
/* ==========================================================================
   Title of page
   ========================================================================== */

/* Title of section
   ========================================================================== */
```

**Wrong:**
```sass
/* Title of section
 **********************/
```

## Quotes

Quotes are optional in CSS. Work & Co chooses to use quotes as it is visually clearer that the string is not a selector or a style property. 

Single quotes are the way forward.

**Right:**
```sass
background-image: url('/img/you.jpg');
font-family: 'Helvetica Neue Light', 'Helvetica Neue', Helvetica, Arial;
```

**Wrong:**
```sass
background-image: url("/img/you.jpg");
font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
```

## SASS/Compass

### Mixins

Make sure to take into consideration the output of using SASS and Compass mixins. They are best used for grouping browser-specific code or as powerful ways to contain functionality. They are generally not a good way to add additional styles to an element as you will be sending duplicate styles across the wire.

### Extends

Should not necessarily be avoided completely, but must be used with caution and with an eye on the outputted CSS. In the wrong hands, the SASS @extend function can cause serious selector bloat and headaches for your fellow developers.

### Vendor-Prefixes

Compass provides [mixins](http://compass-style.org/reference/compass/css3/) for style declartions that require vendor prefixes. It is generally good practice to make use of these wherever possible.

```sass
@include border-radius(10px);
```
