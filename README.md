![CX Partners](http://www.cxpartners.co.uk/wp-content/themes/cxpartners/img/logo/cx-logo.png)

# Front-end Coding Standards and Best Practices

a.k.a. *'How to do webdev at cxpartners'*.

## Overview

These guidelines, principles and standards allow us to:

- produce code of a consistent quality across all projects we undertake
- work concurrently with multiple devs on the same codebase at the same time in the same way
- produce code that is less prone to bugs and regressions, is easier to understand and debug
- write code that supports re-use.

It is required reading for all devs (internal and external) working on a cx web development project.

## Contributing

When considering coding standards we're more interested in **consistency** than **developer freedom**. However this should be considered a living document – it will evolve to reflect the changing needs of the cx dev team and the work we do.

### How to contribute

- Those with access to the repo should create a `proposal/new-guideline` branch where the name reflects the addition or update.
- Otherwise fork the repo and when ready create a pull request.

***

## Global Development Guidelines

### Performance

Page load times (both real and perceived) are a key consideration for users of all browsers and device types.

There are some general things we can do in front-end development:

- Send fewest bytes possible down the wire

- Avoid unnecessary use of `display: none;`

- Keep CSS selectors concise (be wary of SASS nesting)

- Minimise HTTP requests

- Minimise blocking – content should be readable before client side processing

- Lazy load 'supplementary' content (especially images)

### Testing and QA should find no problems

- Code is a craft - make it your responsibility to ensure it is the best it can be; that it's tested, bug free, and adheres to these guidelines. Testing and QA folks aren't responsible for quality - developers are.

- Don't use testers as bug catchers - testers should find no problems after you have committed your code.  When testers find bugs, tickets need to be opened, developers assigned and scheduled in to fix the problem.  This lengthens the time it takes from identifying to resolving a bug.

- Make sure, as much as possible, you have tested your code on a reasonable number of devices so you can catch problems before you commit to the repo.

### You are producing source code

Due to the size of most webdev projects that cxpartners undertakes, and the processes and methodologies we adhere to, there will always be a build process that takes source code and generates built artefacts.

For example:

- SCSS will be compiled to minified and concatenated CSS

- template files will be rendered to HTML

- HTML will be beautified, stripped of comments and references to CSS and JS changed to minified and concatenated versions

- Javascript with be minified and concatenated

This means:

- Don't check generated files into the repo, e.g. CSS files when using SASS

- Always check in configuration files for the tools that you use, e.g. `config.rb` for Compass, `mixture.json` for Mixture.io app

- When including CSS and JS reference the non-minified versions

### Don't Repeat Yourself (DRY)

If you repeat anything that has already been defined in code, refactor it so that it only ever has one representation in the codebase.

If you stick to this principle, you will ensure that you will only ever need to change one implementation of a feature without worrying about needing to change any other part of the code.

### Separation of concerns

Separate *structure* from *presentation* from *behaviour* to aid maintainability and understanding.

- Keep CSS (presentation), JS (behaviour) and HTML (structure) in separate files

- Avoid writing inline CSS or Javascript in HTML

- Avoid writing CSS or HTML in Javascript

- Don't choose HTML elements to imply style

- Where appropriate, use CSS rather than Javascript for animations and transitions

- Try to use templates when defining markup in Javascript

### Write code to be read

> Debugging is twice as hard as writing the code in the first place.  Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it. - Brian Kerninghan.

Follow the principles of ['Keep It Simple, Stupid'](http://en.wikipedia.org/wiki/KISS_principle) (KISS); hard to read or obfuscated code is difficult to maintain and debug.  Don't be too clever; write code to be read.

### Commenting

Explain design or architectural decisions that cannot be conveyed in code alone by adding comments to your code.

Be sure that in conjunction with writing code that adheres to these guidelines, someone can pick up your code and immediately understand it.

Be verbose with your comments but ensure:

- Your comments add something to the code, they don't just repeat what is there

- They are kept up to date, if you change something that has been commented, ensure you up date the comment as well

- If code needs extensive commenting, can it be refactored to make it less complex / easier to understand?

- You focus on *why* rather than *how* - unless your code is too complex, it should be self documenting

Don't leave commented out chunks of code in the codebase. It makes the code look unfinished, and can be confusing for other developers.

### File naming

- Don't use whitespaces in file names - they can lead to problems including text escaping and are harder to read when encoded in URLs

- Use hyphens for word separators

### Identify technical debt

Use code comment annotations to mark parts of your code that require further work. This will allow the measurement and management of technical debt.

Tag | Use
---: | --- | ---
`@todo` |  document tasks to be completed
`@optimise` | mark something that is working, but could be refactored

Don't use `@fixme` (which defines things that are broken) - you shouldn't be committing broken code to the repo.

***
## Styling with CSS & SASS

Our approach to CSS is influenced by Nicole Sullivan's [OOCSS](http://oocss.org/) ideas, and Jonathan Snook's Scalable and Modular Architecture for CSS ([SMACSS](http://smacss.com/)), both of which advocate a general separation of concerns to promote re-usability and prevent code bloat.

### General Guidelines

- Lint your SCSS according to the `scss-lint` configuration file found in the root of all cx projects

- Promote scalable and modular css architecture using the principles defined in the SMACSS style guide

- Utilise [BEM's](http://coding.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/) 'Block', 'Element', 'Modifier' methodology

- Use classes rather than element selectors to de-couple CSS from HTML semantics and ensure that your code doesn't impact how it may be integrated into backend systems

- Make layouts fluid, using variable units of measurement

- Use `id` selectors only when explicitly required – they prohibit re-use, and may need to be re-written during systems integration

- Use short hex values where applicable, e.g. `#fff` instead of `#ffffff`

- Consider using `@warn` and `@debug` directives to aid development, especially within mixins

- Use SCSS variables appropriately to ensure you code is kept DRY

    ```
    // no
    h3 {
      color: white;
    }

    // yes
    $white: #fff;

    h3 {
      color: $white;
    }

    ```

- Each selector and style declaration should be on its own line to help with Git diffs and error reporting.

    ```
    // good
    h3,
    .gamma,
    %gamma {
      @include font-size($h3-font-size);
      line-height: $heading-line-height;
    }

    // not so good
    h3, .gamma, %gamma {
      @include font-size($h3-font-size);
      line-height: $heading-line-height;
    }
    ```

- Don't specify units for zero values, e.g. `margin: 0;` instead of `margin: 0px;`

- Use `0` instead of `none`, e.g. `border: 0;` rather than `border: none;`.

- If you use experimental properties that will require prefixing, it is recommended to use Autoprefixer to post-process the CSS. Autoprefixer can be combined with usage data from [caniuse](caniuse.com) to only output relevant prefixes (e.g., unless you're supporting really early versions of Chrome, you don't need `-webkit-border-radius`), which takes a lot of work out of manual prefixing, and is more intelligent than mixins and libraries.

- Wherever possible, specific page-level styling should be avoided in favour of layout or component modifiers.

- Avoid inline CSS.

- Use shorthand properties:

    ```
    // no
    margin: 1px 1px 1px 1px;

    // yes
    margin: 1px;

    // no
    margin: 0 0 20px 0;

    // yes
    margin: 0 0 20px;
    ```

  It is worth noting that whilst the CSS generated by SASS can be outputted with optimisations applied, we cannot make assumptions about how the code we deliver will be used, and as such creating optimal optimal source code is preferred to relying on processors.

- Write colours in lowercase:

    ```
    // no
    color: #1AB2C0

    // yes
    color: #1ab2c0

    ```

- Omit protocols from external resources to prevent unintended security warnings through accidentally mixing protocols, and for small file size savings:

    ```
    // nope
    .main-navigation {
      background: url(http://d111111abcdef8.cloudfront.net/images/image.jpg);
    }

    // yes
    .main-navigation {
      background: url(//d111111abcdef8.cloudfront.net/images/image.jpg);
    }
    ```

### Depth of applicability

Don't over-specify CSS selectors. Overly specified selectors are difficult to understand and lead to subsequent selectors needing to be of an even higher specificity. (See SMACSS' [depth of applicability](http://smacss.com/book/applicability)):

```
// nope
#sidebar div ul > li {
  margin-bottom: 5px;
}
```

The above example is tightly coupled to the HTML structure which prevents re-use and is brittle - if the HTML needs changing, then the style will break.

### Over qualification

Don't qualify `id`s or classes with tag names.

```
// over qualified id selector
ul#main-navigation {
  ...
}

// over qualified class selector
table.results {
  ...
}
```

As above, you will be binding site structure with presentation making the site harder to maintain and inhibit re-use.

### Commenting

Use comments to:

- Explain design or architectural decisions, to make notes so that any developers modifying, extending or debugging the code can do so understanding your original decisions.

- Divide up groups of declarations using standard block and single line comment formats. (_If you have too many major sections in your partial, perhaps it should be more than one partial!_)

    ```
    /**
     * This is a "block" comment carrying a bit more weight
     * Maybe it has multiple lines
     * It normally announces a new section of some kind
     */

    // Single line explanation of something
    ```

Use multiline comments if you want the comment to be preserved in the compiled CSS.

```
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }
```

### Unit sizing

Use `em`s to size fonts. These will take into account the user's font size setting ([this research from 2006 suggests around 10% of people have changed it](http://archive.oreilly.com/pub/post/more_statistics_on_user_clicks.html)). However, `em`s in font sizes inherit the font size of their parent, so be careful with cascading. If you anticipate needing to size an element or a class inside another element or class that is sized with `em`s (e.g. a `<small>` inside an `<h2>`), and this doesn't cascade nicely, consider using `rem`s, but make sure you always include a pixel fallback for browsers that does not support the `rem` unit.

```
font-size: 12px;
font-size: 1.2rem; /* This line is ignored by IE6, 7 & 8 */
```

Never change the font size of the root element from its default of 100%, which by default equates to 16px. This means avoid the `font-size: 62.5%` hack to ensure that a `(r)em` unit equates to 10px. More elements are 16px than 10px, so we'll have to specify the sizes of fewer things. This is where the cascade excels.

Use unitless line-heights.

Use percentages for fluid layouts and components.

Use pixels to specify the following properties unless percentages make sense, (but above, exercise good judgement). `margin`, `padding`, `top`, `left`, `bottom`, `right`.

```
//  This makes sense
.dropdown-toggle:hover > .dropdown-menu {
  display: block;
  top: 100%; // display just below dropdown-toggle
}

//  This doesn't (magic number alert! - http://csswizardry.com/2012/11/code-smells-in-css/#magic-numbers)
.dropdown-toggle:hover > .dropdown-menu {
  display: block;
  top: 62px; // height of dropdown-toggle at present
}

// This makes sense
.box {
  margin-bottom: 20px;
}

//  This doesn't:
.box {
  margin-bottom: 6.33%; // converted from pixels on the mockup based on box height
}
```

Use `box-sizing: border-box` globally. This makes dealing with multiple units in this fashion a lot nicer.

***

## HTML Markup

### General Guidelines

- Use well-structured, semantic markup.

- Use double quotes on all attributes.

- Use soft tabs, 2 space indents.

- Ensure you write valid HTML.  Check using tools such as the [W3C Markup Validation Service](http://validator.w3.org/).

- Do not omit [optional tags](http://www.whatwg.org/specs/web-apps/current-work/multipage/syntax.html#syntax-tag-omission).  It may be unclear whether a tag has been deliberately omitted, or if it has been left out accidentally.

- Although unquoted attributes are supported, always quote attribute values.

    ```
    <input type=text class=form__field form__field--string /> <!-- uh oh -->
    ```

- Omit protocols from external resources to prevent unintended security warnings through accidentally mixing protocols:

    ```
    <!-- Don't do -->
    <script src="http://www.google.com/js/gweb/analytics/autotrack.js"></script>

    <!-- Do -->
    <script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
    ```

### Doctype

Use the HTML5 doctype to indicate that you are serving HTML5 content.  Using this doctype ensures your browser's layout engine uses 'standards' (or 'no-quirks') mode (rather than 'quirks' or 'almost-standard' modes).

Any browsers that don't currently support HTML5 will enter this mode and interpret non-HTML5 in a compliant way whilst ignoring new, unsupported features.

```
<!DOCTYPE html>
```

### Write semantic markup

Ensure the markup you write is relevant and has meaning in the context of the content it is being applied to. Don't use markup to infer style.

```
<!-- bad: -->
<div class="mainHeading">My Blog</div>
<br /><br />
... content ...
<br /><br />

<!-- bad: -->
<h1>I want to draw attention to this as I am important</h1>
<h1>and so am I</h1>

<!-- good: -->
<h1>My Blog</h1>
<p>
   ... content ...
</p>
```

### Input labels

Use `label` tags for `input` fields so that the input element acquires focus when the label is clicked.

When using labels, try to use the wrapping pattern rather than the `for` attribute so we can avoid using  `id`s which may interfere with integration with backend systems:

```
<label>Address
  <input type="text" name="address" />
</label>

<label for="address">Address</label>
<input type="text" id="address" name="name" />
```

### Boolean attributes

Use the single word syntax for boolean attribute values to aid readability, reduce clutter and prevent unnecessary bytes going down the wire.

The presence of the attribute itself implies that the value is "true", an absence implies a value of "false":

```
// old hat
<option selected="selected">value 1<option>

// cutting edge
<option selected>value 1<option>
```

***

## Behaviour & JS

*Note: We use jQuery heavily in our projects, and so many examples are written as jQuery.*

### General guidelines

- Use soft-tabs with a two space indent.

- Never use `eval`.

- All projects will contain a `.jshintrc` file in the root.  This will define the expected coding standards for the project, enforced by JSHint.

- Don't use CoffeeScript - it's an abstraction too far.  Javascript is far from a perfect language, but learn how to deal with the issues of Javascript using recognised and accepted patterns and adhere to best practice guidelines.

- Avoid applying styles with Javascript, preferably add or remove classes.  Keep style in CSS to make it easier to maintain and debug.

    ```
    //  no good
    <input type="text" style="display:none;" name="address_1" />
    ```

- Use SMACSS `is-*` state rules to apply state to elements, for example `addClass('is-hidden')` `addClass('is-collapsed')`.

    ```
    //  all good
    <input type="text" class="is-hidden" name="address_1" />
    ```

- Opt in to using a restricted variant of JavaScript using `use strict` so that errors that should be thrown are.

- Avoid inline Javascript in HTML markup.

- Don't recreate functionality that may already be present in a utility library that is already in use.

- When using a third party library, ensure it has `/*!` in the file comment block so that licenses are preserved when the Javascript libraries are minified.

    ```
    /*!
     * enquire.js v2.1.0 - Awesome Media Queries in JavaScript
     * Copyright (c) 2013 Nick Williams - http://wicky.nillia.ms/enquire.js
     * License: MIT (http://www.opensource.org/licenses/mit-license.php)
     */
    (function(t,i,n){var e=i.matchMedia;"undefined"!=typeof module&&module.export
    /*!
     * jQuery Validation Plugin 1.11.1
     *
     * http://bassistance.de/jquery-plugins/jquery-plugin-validation/
     * http://docs.jquery.com/Plugins/Validation
     *
     * Copyright 2013 Jörn Zaefferer
     * Released under the MIT license:
     *   http://www.opensource.org/licenses/mit-license.php
     */
    !function(t){t.extend(t.fn,{validate:function(e){if(!this.length)return e
    ```

- Send data down the wire and theme on the client, rather than sending HTML when making AJAX requests.

- Always use parentheses in blocks to aid readability:

    ```
    // good...
    while (true) {
      shuffle();
    }

    // not good...
    while (true)
      shuffle();

    // also not good...
    while (true) shuffle();
    ```

- Use the `===` comparison operator to avoid having to deal with type coercion complications.

- Use quotation marks consistently, e.g. only use single or double quotes throughout your code, don't mix.

- Use `event.preventDefault();` instead of `return false;` to prevent default event actions. Returning a boolean value is semantically incorrect when considering the context is an event.

    ```
    //  returning false doesn't make sense in this context
    $('.myElement').on('click', function(e) {
      // do stuff
      return false;
    });

    // use preventDefault
    $('.myElement').on('click', function(e) {
      e.preventDefault();
      // do stuff
    });

    ```

### Don't blindly follow 'patterns'

Although following patterns helps us deal with the spaghetti that is Javascript, some patterns become anti-patterns after time:

- function scope handling with `that = this` / `self = this` pattern:

    ```
    // Ohhhh I would do anything for scope, but I won't do that = this - @jaffathecake
    var person = {
      name: 'Alice',
      greet: function() {
        var self = this;
        setTimeout(function() {
          console.log('Hello ' + self.name); // 'Hello Alice'
        }, 2000);
      }
    };
    person.greet();

    // better:
    var person = {
      name: 'Alice',
      greet: function() {
        setTimeout(function() {
          console.log('Hello ' + this.name); // 'Hello Alice'
        }.bind(this), 2000);
      }
    };
    person.greet();

    // or in jQuery:
    var person = {
      name: 'Alice',
      greet: function() {
        setTimeout($.proxy(function() {
          console.log('Hello ' + this.name); // 'Hello Alice'
        }, this), 2000);
      }
    };
    person.greet();

    ```

- single var pattern:

    ```
    // single var pattern:
    var myVar1 = 1,
        anotherVar2 = 'test',
        andAnotherVar = true;

    // is this cool?
    var myVar1 = 1,
        anotherVar2 = 'test' // oops, Automatic Semicolon Insertion just popped a ; on this...
        andAnotherVar = true; // is now global

    // better?
    var a = 1
      , b = 2
      , sum = a + b
      , myobject = {}
      , i
      , j;

    // good old dependable multiple var pattern:
    var myVar = 1;
    var anotherVat2 = 'test';
    var andAnotherVar = true;

    ```

### Avoid excessive function arguments

If a function requires more than three arguments, considering refactoring to use a configuration object:

```
// bad
var myFunction1 = function(arg1, arg2, arg3, arg4) {}

myFunction1('firstArgument', argument2, true, 'My Third Argument');

// good
var myFunction2 = function(config) {}

myFunction2({
  arg1: 'firstArgument',
  arg2: argument2
  arg3: true,
  arg4: 'My Third Argument'
})
```

Configuration objects:

- allow for optional parameters,

- are easier to read,

- are easier to maintain.

### Naming conventions

Although we can infer that `_oA` is a private object, we know nothing about what value it will contain.

Use descriptive yet concise variable names for long-lived variables.

Avoid using what might be considered reserved variable names out of context.  For example, `e` in jQuery is considered an event object.  Don't use it for anything else.

For temporary variables, i.e. those which are used to store short-lived values, e.g. variables used for iteration, it is ok to use non-descriptive names, e.g. `i, j, k`.

### Namespace events

jQuery supports namespaced events.  In order to make your scripts play nicely with other libraries, ensure any events you bind to have a namespace so that if you need to unbind it, it won't affect any other scripts that use the same event.

```
// not cool...
$(window).bind('resize', function(e) {
  // do something
});

$(window).unbind('resize');

// you're ok...
$(window).bind('resize.myPlugin', function(e) {
  // do something
});

$(window).unbind('resize.myPlugin');
```

### Commenting

Comment your code.  There are two reasons why you should comment:

- Inline comments explain design or architectural decisions that cannot be conveyed in code alone.  These are mainly for the benefit of other developers modifying, extending or debugging the code, but also for you - you may return to the code a week later and wonder how it works.

    ```
    /**
     * I am a nicely formatted comment
     */

    // I am a nicely formatted brief one-line comment

    for ( var i=0; i<10; i++); // uh-oh don't put comments at the end of lines

    /* ===================================================
     * uh-oh don't make up your own comment format
     * ==================================================*/

    //  This is a long comment
    //  that shouldn't really
    //  be using the one-line
    //  comment syntax.

    ```

- For API documentation for users who want to use your libraries / plugins etc., without reading the code.  Use JSDoc standards to comment files and functions:

    ```
    /**
     * @file A jQuery UI Widget to build product swatches
     * @author Joel Mitchell
     * @name $.cx.productSwatch
     * @dependencies: jQuery, jQuery UI widget factory (mockjax for testing)
     */
    ```

    ```
    /**
     * Event handler to close the menu
     * @param {Event} e jQuery event object
     */
    var myFunction = function(e) {};
    ```

### Localisation

Even if the project doesn't require it at the point at which you are working on it, make sure you code is extensible - if you need to include text strings in Javascript make sure they are either:

- wrapped in a localisation function:

    ```
    // localisation function
    _t('Click me')

    /**
     * Future proofing
     */
    function _t(str) {
      return str;
    }

    // some time in the future...
    function _t(str) {
      var languages = {
        "fr": {
          'Click me' => 'cliquez sur moi'
        }
      };

      return language[currentLanguage][str];
    }

    ```

- or defined in a configuration object:

    ```
    $.widget('formToggle', {

      options: {
        'btnLabel': 'Click Me'
      };

      ...

      _create: function() {
        $('.button').val(this.options.btnLabel);
      }

      ...

    })(jQuery);

    ...

    $('form').formToggle({
      'btnLabel': 'cliquez sur moi' // i'm French now.
    });

    ```

### Plugin Development

Break down Javascript into components implemented (where appropriate) as jQuery plugins.

The benefits of breaking down larger JS projects into smaller 'single-purpose' chunks are well known, but include the promotion of maintainability as it is easier to understand and update a small piece of single purpose code.

Using discrete jQuery plugins to achieve this allows us to encapsulate functionality and abstract away the implementation so other developers don’t necessarily need to know how it works, only how to use it. This allows us to better test the functionality in isolation and to structure  projects.

We suggest the use of jQuery UI's [‘Widget Factory’](http://api.jqueryui.com/jQuery.widget/) plugin component as the basis of plugins.

It gives us a well defined and consistent API to work to, and being stateful, allows us to better deal with the challenges of modern website development.

- Prefix the filename of your plugin with `jquery.cx` e.g. `jquery.cx.responsive-image.js`.

### Avoid HTML in JS

Long strings of text and HTML in code are hard to maintain and understand.  Considering using a template library such as Moustache.

```
// configuration object
options: {
  'errorTemplate': '<div class="error-message"><p>{{error-message}}</p></div>
}

```

Where possible, put templates in HTML or seperate files for a clear separation of behaviour and presentation:

```
<script id="entry-template" type="text/x-handlebars-template">
  <div class="error-message">
    <strong>The following errors have occurred:</strong>
    <p>{{error-message}}</p>
  </div>
</script>

$('form').validationPlugin({
  'errorTemplate': $('#entry-template').html()
});

```

### Break code into separate lines

Where applicable, ensure code is written on separate lines to aid Git diffs and error reporting:

     // good
    page.setViewport({
      width: 1280,
      height: 1024
    });

     // not so good
    page.setViewport({width: 1280, height: 1024});
