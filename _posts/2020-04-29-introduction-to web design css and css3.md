---
layout: post
title:  "Introduction to Web Design - CSS and CSS3 "
author: sma
categories: [ WEB, WEBDESIGN, HTML, HTML5, CSS, CSS3 ]
image: assets/images/posts/carlos-muza-hpjSkU2UYSU-unsplash.jpg
description: "Learn web design html, html5, css and css3"
tags: [featured]
---

## Introduction to CSS

We use *HTML* to describe contents of our web pages and not to format this content, to overcome this limitation *W3C* designed a new language known as **CSS** *(Cascading stylesheet)*.

## Understanding CSS Syntax

**CSS** is composed of rulesets, to understand CSS ruleset let's first see an example.


```
p {
    color: blue;
}
```

Let's understand the parts of css syntax one by one.

### Selector

Selector in css is used to refer html element, in above example `p` is the selector which will point to all `paragraph or p` elements in html document.

### Declaration

Declaration is the body of css block, in above example `{ color: blue; }` is the declaration. *CSS* declaration is composed of one or more pairs of `property` and `value`. In above example `color` is the property with an assigned value of `blue`. Each pair of `property` and `value` in *CSS declaration* is separated or closed with a semicolon `;`.

## Understanding CSS Selectors

As described above, *CSS Selectors* are used to *refer* or *select* html elements and this selection can be made using different approached, in following section we will explore different methods to use *CSS selectors*.

### Element Selector

Element selectors are based on *HTML element* name.

### Examples

Following CSS snippet defines an element selector for all *h1* elements.
```
h1 {
 color: red;
}
```

Following CSS snippet defines an element selector for all *div* elements.
```
div {
 color: red;
}
```

Following CSS snippet defines an element selector for all *p* elements.
```
p {
 color: red;
}
```


### Id Selector

Id selectors are used to select *HTML* elements with a specific `id` attribute.

### Examples

Following CSS snippet defines an id selector to select all *HTML* elements having value `my1` for `id` attribute. 

```
#my1 {
 color: red;
}
```

This css block / style will be applied to elements like.

```
<p id="my1"> Para 1 </p>
```


### Class Selector

Class selector is used to select elements with a specific class attribute.


### Examples

Following CSS snippet defines an id selector to select all *HTML* elements having value `red` for `class` attribute. 

```
.red {
 color: red;
}
```

This css block / style will be applied to elements like.

```
<p class="red"> Para 1 </p>
```

You can also apply multiple *CSS class* style blocks on a single element.

```
.big {
    font-size: 24pt;
}
```

Both style blocks `red` and `big` will be applied to `p` element used in following code snippet.

```
<p class="red big"> Para 1 </p>
```

## Using CSS

You can use *css* styles using following three methods.

### Inline style

Inline style can be applied directly to an html element using `style` attribute.

```
<p style="color:red;">Hello World</p>
```

### Internal style

Inline styles are enclosed in `<style> ... </style>` block within an html document. You can add `<style> ... </style>` block anywhere in html document, however usually it is defined in `<head> ... </head>` block.

```
<!DOCTYPE html>

<html>
    <head>
        <style>
            p {
                color: green;
            }        
        </style>
    </head>
    <body>
        <p>Hello how are you</p>
    </body>
</html>
```

or


```
<!DOCTYPE html>

<html>
    <head>
        <style>
            p {
                color: green;
            }        
        </style>
    </head>
    <body>
        <p>Hello how are you</p>

        <style>
            .mystyle {
                color: red;
                font-size: 22pt;
            }        
        </style>

        <p class="mystyle" > This is with my style </p>

    </body>
</html>
```

### External style

Instead of writing the *CSS style* directly in html file, you can define you *css styles* in a seprate text file and then include this css file in your html as an external style.

**style.css**

```
.mystyle {
    color: red;
    font-size: 22pt;
}        

.greeny {
    color: green;
}        
```


**index.html**

```
<!DOCTYPE html>

<html>
    <head>
        <link rel="stylesheet" type="text/css" href="style.css">
    </head>
    <body>
        <p class="greeny">Hello how are you</p>

        <p class="mystyle" > This is with my style </p>

    </body>
</html>
```


## CSS Cascading Order

CSS styles are cascaded in following order, having one the highest and 3 with lowest priority.

1. Inline style.
2. Internal and external style.
3. Default style of Web Browser.


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
