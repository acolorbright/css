# ACB CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass, based on Airbnb's [Styleguide](https://github.com/airbnb/css)*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [ITCSS and BEM](#itcss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
    - [Sizing units](#sizing-units)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
1. [CSS-in-JS](#css-in-js)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ back to top](#table-of-contents)**

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names.
  - Underscores are okay if you are using BEM (see [ITCSS and BEM](#itcss-and-bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific code

### ITCSS and BEM

We encourage the combination of ITCSS and BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**ITCSS**, or “Inverted Triangle CSS”, is an approach for writing CSS that divides your styles up into layers from least to most specific (hence “Inverted Triangle”). It incorporates the concept of “object-oriented CSS”, which encourages you to think of your styles as a collection of “objects”: reusable, repeatable snippets that can be used independently throughout a website. ITCSS further makes a distinction between “components” (more specific) and “objects” (more generic).

The layers are as follows:
  1. Settings, e.g. `_settings.colors.scss`, `_settings.spacing.scss`
  1. Tools, e.g. `_tools.media-queries.scss`, `_tools.typography.scss`
  1. Generic, e.g. `_generic.reset.scss`, `_generic.fonts.scss`
  1. Elements, e.g. `_elements.page.scss`, `_elements.forms.scss`
  1. Objects, e.g. `_objects.block.scss`, `_objects.panel.scss`
  1. Components, e.g. `_components.button.scss`, `_components.markdown-content-block.scss`
  1. Trumps/Alterations/Utils, e.g. `_alterations.alignment.scss`, `_alterations.hacks.scss`

For details, see Harry Roberts' introduction to ITCSS on [Creative Bloq](https://www.creativebloq.com/web-design/manage-large-css-projects-itcss-101517528).

Related reading:
  * Jonathan Snook's [SMACSS](https://smacss.com/)
  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing ITCSS.

We recommend using namespaces with ITCSS and BEM, so that objects have a `o-` prefix, components have a `c-` prefix, and utils have a `u-` prefix.

  * CSS Tricks' [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
  * Harry Roberts' [More Transparent UI Code with Namespaces](https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)

**Example**

```jsx
// MarkdownContentBlock.jsx
function MarkdownContentBlock() {
  return (
    <section class="o-block c-markdown-content-block">

      <h1 class="c-markdown-content-block__title">Lorem Ipsum</h1>

      <article class="c-markdown-content-block__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </article>

    </section>
  );
}
```

```css
/* _components.markdown-content-block.scss */
.c-markdown-content-block { }
.c-markdown-content-block--featured { }
.c-markdown-content-block__title { }
.c-markdown-content-block__content { }
```

  * `.c-markdown-content-block` is the “block” and represents the higher-level component
  * `.c-markdown-content-block__title` is an “element” and represents a descendant of `.c-markdown-content-block` that helps compose the block as a whole.
  * `.c-markdown-content-block--featured` is a “modifier” and represents a different state or variation on the standard `.c-markdown-content-block`.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="c-btn c-btn--primary js-share">Share</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border. (Both are valid; we prefer choosing one of the two for consistency and `0` is shorter so wins.)

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

### Units

Prefer `rems` to other sizing units. Borders may be sized in `px`. Spacing of text may be sized in `em`s.


**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    Properties should be roughly ordered according to the [SMACSS recommendation](https://smacss.com/book/formatting#grouping) of:

      1. Box
      1. Border
      1. Background
      1. Text
      1. Transforms/Animations
      1. Other

    `@include` declarations should be inserted at the appropriate point, e.g.

    ```scss
    .c-feature {
      position: absolute;
      @include spacing('pv', '1/2');

      border: 0;

      @include font-size('s');

      transform: translateX(-50%) translateY(-50%);
    }
    ```

2. Media queries

    Inserting media queries at the end makes it easier to understand how components change with screen size.

    ```scss
    .c-feature {
      position: absolute;
      @include spacing('pv', '1/2');

      border: 0;

      @include font-size('s');

      transform: translateX(-50%) translateY(-50%);

      @include media-query('m') {
        // ...
      }
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .c-feature {
      position: absolute;
      @include spacing('pv', '1/2');

      border: 0;

      @include font-size('s');

      transform: translateX(-50%) translateY(-50%);

      @include media-query('m') {
        // ...
      }

      &__icon {
        margin-right: 1rem;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names.

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[⬆ back to top](#table-of-contents)**


## CSS-in-JS

We are experimenting with moving away from SCSS and towards CSS-in-JS but have not settled on a standard solution yet. In the meantime, see our notes from our experiments in the [ACB Handbook Wiki](https://github.com/acolorbright/handbook/wiki/Styles#css-in-js).

### More to come...

**[⬆ back to top](#table-of-contents)**
