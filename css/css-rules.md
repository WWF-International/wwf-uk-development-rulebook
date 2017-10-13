# WWF-UK CSS style guide

This style guide sets out the internal practices for CSS. The objective for this is to make our stylesheets look as if they were written by one person, with consistent naming, formatting, commenting etc.

> Like our JavaScript style guide, this should be considered as both draft and canonical. Draft because I'm open to a debate on the practices defined. Canonical because in the absence of that debate the guidance here should be reflected in your code.

Credit where credit is due; this is basically WWF-flavoured [BEM](https://en.bem.info)

Recommended reading:
- **[MindBEMding - getting your head 'round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)** - if you only read one, read this one.
- [BEM key concepts](https://en.bem.info/method/key-concepts/)
- [Naming conventions](https://en.bem.info/method/naming-convention/)

## General principles

> "Part of being a good steward to a successful project is realizing that writing code for yourself is a Bad Idea™. If thousands of people are using your code, then write your code for maximum clarity, not your personal preference of how to get clever within the spec." - Idan Gazit

- Don't try to prematurely optimize your code - keep it readable and understandable.
- Make sure it's explained, commented and well named.
- All code in any code-base should look like a single person typed it, even when many people are contributing to it.
- Strictly adhere to the style syntax.
- If in doubt when deciding upon a style - take a look at existing code and use common patterns.


### tl;dr
- BEM (**b**lock__**e**lement--**m**odifier)
- names-use-hyphens
- clarity over compactness
- never use #id
- each selector and property on its own line
- indent with spaces - multiples of four
- ems and %


## Structure
This may seem picky, but it really does help make sure that the code remains legible - and that people in the far distant future can tell what we were doing.

- Spaces are used to indent in multiples of four
- One blank line between blocks of code within a section
- Two blank lines between different sections
- One selector per line, followed by a comma or an opening curly brace
- The opening curly brace should have a space before it
- Properties and their values should be
    - indented with one indentation (four spaces)
    - on the same line as each other
    - always closed with a semicolon - including the last one
- The closing curly brace is indented at the same level as the opening selector

For example:

```css
.selector-one,
.selector-two,
.selector-three {
    background: lawngreen;
    color: lightyellow;
}
```

But not:

```css
.selector-one, .selector-two, .selector-three{
    background: lawngreen;
    color: lightyellow
}
```

or
```css
.selector-one,
.selector-two,
.selector-three{background: lawngreen;
    color: lightyellow
    }
```

## Modular coding
Reusing code is great, since it minimises file size - but further down the road can end up with a spaghetti. Even if it means repeating things, keep styles specific to a module or a shared style.

BEM is ideal for this, as it allows us to see just from the CSS what styles do what where.

**Correct:**

```css
.menu {
    background: goldenrod;
}

.menu__tab {
    border-radius: 0.5em 0.5em 0 0;
}

.menu__tab--active {
    background: green;
}

.header {
    background: goldenrod;
}

.search {
    width: 25%
}

.search__submit {
    background: goldenrod;
}
```


**Incorrect:** too broad

```css
img {
        border: 1px solid grey;
        padding: 1px;
    }
```

**Incorrect:** premature optimisation - this path leads to convoluted code.

```css
.menu,
.header,
.search__submit {
    background: goldenrod;
}

.search {
    width: 25%
}

.menu__tab {
    border-radius: 0.5em 0.5em 0 0;
}

.menu__tab--active {
    background: green;
}
```


## Selectors

> ‘With great power there must also come great responsibility’ - Uncle Ben

Broad selectors give efficient and easily understandable code, but can lead to unseen consequences. Very specific selectors can target accurately, but tend to lead to a cluttered and large stylesheet.

**Don't use `#id` for styling.** They're too specific and give long term pain. They lack reusability, tend to lean towards use-once code, and the only way to override them is with the dreaded `!important`. So we don't use `#id` for styling.

**Broad selectors - such as `figure`, `p`, `i` etc - are okay for foundation styles**. This means only for setting up a base to build apon. If possible avoid this.

We aim to keep semantic markup separate from style. Most of your styling should be done using classes. They’re flexible enough to be broad and specific, allow reusing without creating a spaghetti of CSS code.

However `p`, `a`, `strong` `form`, `small` etc should also be thought of as foundation styles to be built upon. After all, it'd be daft to attach a class to every single `p` or `strong` tag!

For example:

```css
/* foundation paragraph style */
p {
    color: steelblue;
    font-size: 1em;
    padding: 0 0 1em 0;
}

/* style specific to paragraphs within a figure */
.content__figure p {
    padding-bottom: 0.25em;
}

.aside__figure p {
    padding-left: 0.5em;
}
```

- When naming classes, use the BEM syntax.
- Make classes human readable - describe the element(s) that they style or the function they carry out:
    - `.search`
    - `.search__input`
    - `.search__submit--loading`
- Always use lowercase and separate words with hyphens - no `.CamelCase` and `.under_scores`:
    - `.adoption-footer__sumartran-orang-utan`
- Attribute selectors should use single quotes - increases legibility for humans.
- Target only the class, not the element and class - for example: `.comment-form`, not `div.comment-form`
- Be careful of selector supporter in older browsers - for example, `input[type='password']` or `input:not([type='submit'])not:([type='password'])`. Use them, but use with caution.

**Correct:**

```css
header,
footer {
}

.visually-hidden {
}

input[type='submit'] {
}

input[type='number'] {
}
```


**Not:**
```css
div#logo {
}

#logo {
}

h1.logo {
}
```

## Properties
- 0.5em, not .5em - as makes it easier to distinguish between 0.5em and 5em
- Double quotation marks for attribute selectors, property values, and URI values.

```css
header {
    font-family: "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
    background-image: url("dancing-cat.gif");
}

header::before {
    content: "Pseudo content";
}
```

## Property ordering
Alphabetic. (If using Sublime Text, highlight the properties and press F9)

```css
p {
    background: #fff url('background.jpg') no-repeat 50%;
    line-height: 2;
    margin: 0;
    padding: 0 1em 1em 0;
    position: relative;
    width: 50%;
    z-index: 1;
}
```

A special mention for vendor prefixes; they're alphabetacised as if they don't have the vendor prefix. The W3C property comes last so that it isn't overriden. They're indented with an extra tab and then alphabetised based on the vendor prefix.

```css
        -moz-transition: all 300ms linear;
        -ms-transition: all 300ms linear;
        -o-transition: all 300ms linear;
        -webkit-transition: all linear;
    transition: all 300ms linear;
```

If you’re using Sass or Less, then use a variables and mixins file as a repository for vendor prefixed styles. You can then just use `.transition(all 300ms linear);` or `@include transition(all 300ms linear);` to keep the code clean.


## Property values - Pixels vs. Ems
The property preference hiererchy goes:

1. `em` or `%`
2. Nothing else

Whether `em` or `%` is used is dependant on the situation, but the important thing about these is that they're flexible and don't block the stylesheet's cascade.

Pixels aren't needed - preprocessors can do the division for you to convert `px` into the `em` equivalent.

For example, in Less or Sass:

```css
/* method: */
width: ( [pixel number] / [parent element em size]em );

/* for example: */
width: ( 20 / 16em );

/* would output */
width: 1.25em;
```

If you don't use preprocessors, then still use `em` - the numbers will just neeed to be calculated by hand.

*The reasoning:* using either pixels, points, or root ems block the cascade of inheritance. Using `em` or `%` we allow elements to inherit characteristics - which makes designing responsively much lighter, as we're not rewriting the CSS for every viewport size.

This can seem frustrating if you're unfamiliar with the C of CSS, but the killer advantage is that you can just change the root font size and everything else will change in proportion. This means leaner, faster code when designing for different screen sizes.


## How to comment
Comments within the code should be easily visible, but not create muddle. With this in mind, these are the following rules for commenting within CSS.

For consistancy, the CSS style comments - `/* */` - are always used, even when using preprocessors.

**To do lists and noticed bugs that aren't yet fixed are incredible useful comments** - especially when using a repository such as Git or SVN. Please make notes within the code to help the rest of the organisation keep up with you. As with JavaScript, use the prefixes `/* FIXME */` and `/* TODO */` where appropriate.

At the top of the style.css (see *File structure*), this description should go as a minimum. Note the exclamation mark, which (should) preserve the comment after minification.
```
/*! ============================================================================

Project:        Name
Version:        [major].[minor].[bugfixes]
Last change:    Date (quick comment)
Assigned to:    Person, person, person

============================================================================= */
```

The versioning follows semantic versioning[^!semanticversioning]

Any other descriptors are welcome - contact details, to do list etc.

**Headings** for sections of code should be highlighted with single lines:

```
/* -----------------------------------------------------------------------------
    Heading
    - small description of what's going on here
    - another point, maybe a bugfix with a FIXME
    - TODO if needed
----------------------------------------------------------------------------- */
```

**Individual comments in the code** don't have the single or double lines.

If referring to the whole selector, then the comment should sit inside it.
```css
.standfirst p {
    /* this is to make the first paragraph into a standfirst */
    font-weight: bold;
}
```

If referring to an individual property, it should sit on the same line after the semicolon:

```css
    font-size: 1.25em; /* equivalent to 20px */
    margin-top: 1.48em; /* FIXME magic number */
```


## Media Queries

If Internet Explorer 8 (or older) support is needed, then go desktop first. Any desktop styles then won't have be duplicated in an Internet Explorer only stylesheet.

Otherwise, if Internet Explorer 8 (or older) support isn't required, then go mobile first.

The breakpoints should be in `em`, as this provides the widest level of consistent, bug-free support. Using `ems` in media queries doesn't use the root setting - it uses the browsers default font size, usually 16px.

Any styles that are triggered by media queries should be placed within the stylesheets - not in their own stylesheet.

For example, within the *modules* stylesheet:
```css
[ Small screen rules for specific module here ]

@media (min-width: 37.5em) {
    /* break point is equivalent to 600px (37.5 x 16) */
    [ Medium screen rules for specific module here ]
}

@media (min-width: 48em) {
    /* Break point equivalent to 768px (48 x 16) */
    [ Medium screen rules for specific module here ]
}

@media (min-width: 80em) {
    /* Break point equivalent to 1280px (80 x 16) */
    [ Large screen rules for specific module here ]
}
```

If edits are required to a specific module, then everything for that module is in one place - rather than spread over several different stylesheets.

## Best Practices
Stylesheets tend to get long in length. Focus slowly gets lost whilst intended goals start repeating and overlapping. Writing smart code from the outset helps us retain the overview whilst remaining flexible throughout change.

- If you are attempting to fix an issue, try to remove code before adding more.
- Magic Numbers are unlucky. These are numbers that are used as quick fixes on a one-off basis. Example:

```css
.box {
margin-top: 37px;
}
```
- Know when to use the height property. It should be used when you are including outside elements (such as images). Otherwise use line-height for more flexibility.
- Do not reastate default property and value combinations (for instance `display: block;` on block-level elements [^!HTML5block].


## Footnotes

[^!semanticversioning]: [Semantic versioning](http://semver.org/)

[^!HTML5block]: An exception is for HTML 5 elements, such as `section`, `header` etc. These should be display: block at the start of the stylesheet for older versions of Firefox. But then, you probably knew that - or are using something nifty like *normalize.css* which does that anyway.
