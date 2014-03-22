Title CSS
=========

Title CSS is an organizational technique geared to help you write beautiful and maintainable CSS. Similar to [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/), Title CSS aims to make CSS more readable by giving visual cues.

The trick to Title CSS is simple. For any global CSS class use a title case name (uppercase the first letter). For any modifiers or descendant class begin the class with a lowercase letter.

Here is an example of the markup:
```html
<div class="Title modifier">
    <p class="descendant">
</div>
```

Here is how you would target these elements in CSS:
```css
.Title {}
    .Title.modifier {}
    .Title .descendant {}
```

This is why it works
--------------------
Block identifiers or "Title" classes create a scope for all the descendent classes within the block. Descendant classes can be repeated in other Title blocks without style collision.

HTML class names are *case-sensitive*. This is mentioned in the [HTML4 spec](http://www.w3.org/TR/REC-html40/struct/global.html#h-7.5.2). L. David Baron of Mozilla interprets the CSS and HTML specs [here](http://dbaron.org/css/test/casesens).

> Still not convinced? Try any browser since Netscape 6. Still afraid of utilizing case-sensitivity? Just make a rule not to reuse Title classes as a lowercase descendant class.

Enjoy the benefits of BEM CSS such as the block style grouping of elements and the security of keeping selectors from interfering with each other.

How does it help?
-----------------
* Write CSS classes in a more natural manner.
* CSS selectors resemble the written language since sentences start with an uppercase letter.
* Shorter class names are faster to type and easier to scan.
* Title case classes are easy to spot in the markup. To see what a lowercase descendant class belongs to, just traverse up the nodes for a Title class.

Pitfall and workaround
------------------------
Title CSS may have issues when you use a Title block to contain other Title blocks. If a containing Title block has the same descendant selector class as one that it envelopes than there will be a conflict, in which case you should use [child selectors](http://www.w3.org/TR/CSS21/selector.html#child-selectors) in Title blocks that act as containers.

To demonstrate the issue:
```html
<div class="Container">
    <header class="header"></header>
    <main class="body">
        <section class="Title">
            <div class="header"></div>
            <div class="body"></div>
        </section>
        <section class="Title">
            <div class="header"></div>
            <div class="body"></div>
        </section>
    </main>
</div>
```
```css
.Container {}
    .Container .header {}
    .Container .body {}
.Title {}
    .Title .header {}
    .Title .body {}
```

The solution:
```css
.Container {}
    .Container > .header {}
    .Container > .body {}
```

Terminology
-----------

*Examples are in SCSS or CSS*

### Modules
A module the grouping of related CSS selectors that make up a block. Each selector in a module begins with a Title class. Modules are described in more detail in [SMACSS](http://smacss.com/).

```scss
.Module {
    > .relatedChild {}
    .relatedDescendant {}
}
```

### Title Classes
A title class is the capitalized class that creates the scope for any descendant classes in a module.

```scss
.TitleClass {
    .scopedElement {}
}
```

### Modifiers
A modifier is a class that can be applied to another class to change the state of that class. Modfiers are usually appended to title classes even if they only affect descendant elements in the module. Title CSS is about making selectors readable so begin modifiers with "is" or a short preposition.

```html
<div class="Slider isEnabled"></div>
<ul class="List of5"></ul>
<div class="Checkout isActive"></div>
<ul class="Group ofProducts"></ul>
```

### Objects [(OOCSS)](http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)
Objects are capitalized in Title CSS just like title classes becuase they are both occupying the global CSS namespace. Objects are different from Modules in that they typically don't have descendants and they are usually combined with other classes. To distinguish objects from title classes prefix the object class name with an article such as "A" or "Un". Suggestions for prefixing singleton objects would be "The", "El", or "Le".

```html
```

Performance
-----------
Title CSS does not help you write the most performant selectors. The issue is that browsers typically read selectors from right to left. If there are commonly used classes or elements at the end of the selectors then the browser will most likely have to do more work to read through the styles. Best practice is to keep the descendant selectors as shallow as possible.

Have a look at this example that can improve performance as well as maintainability:
```css
.Title .withToo .many .selectors {}
```
```css
.Title .selectors {}
```

SCSS and other pre-processer syntaxes
-------------------------------------
Sass is an excellent tool to help write Title CSS. With the nesting ability found in pre-processing languages, it is easy to identify new Title blocks in the stylesheet.

```scss
.Title {
    &.modifier {}

    .descendant {}

    > .tightlyBound {}
}
```