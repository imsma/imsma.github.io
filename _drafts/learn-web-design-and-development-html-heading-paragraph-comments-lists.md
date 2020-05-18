---
layout: post
title:  "Learn Web design and development - HTML Heading, Paragraph, Comments, Lists"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/posts/markus-spiske-9wTPJmiXk2U-unsplash.jpg
imageAttribution: Photo by Markus Spiske on Unsplash
description: "In this article we will learn about HTML tags to create core parts of a web page. We will explore how to create headings, paragraphs and lists using HTML tags."
tags: [featured]
---


## HTML Headings

In *HTML* we have six types of **headings*, `h1`  to `h6`.

```
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

Sample output of about *html code block* will is.

---
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
---

## Paragraphs in HTML

is block


```
<p>
Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eveniet quae culpa laudantium alias similique esse ipsum ea consequuntur, quidem eaque minima aliquam non explicabo laborum sint, ad libero fuga inventore.
</p>
```

will show

<p>
Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eveniet quae culpa laudantium alias similique esse ipsum ea consequuntur, quidem eaque minima aliquam non explicabo laborum sint, ad libero fuga inventore.
</p>



## Span element <span> </span>

is inline


```
<span>Hello</span>
<span>World</span>
```

will show

<span>Hello</span>
<span>World</span>



## Lists in HTML

There are three types of lists in HTML *Ordered or Numbered Lists*, *Unordered or Bulleted Lists*, *Description or Definition Lists*,...


### Ordered List in HTML / HTML Numbered List

```
<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>TypeScript</li>
</ol>
```

<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>TypeScript</li>
</ol>

https://www.javatpoint.com/html-ordered-list





ol type

Type	Description
Type "1"	This is the default type. In this type, the list items are numbered with numbers.
Type "I"	In this type, the list items are numbered with upper case roman numbers.
Type "i"	In this type, the list items are numbered with lower case roman numbers.
Type "A"	In this type, the list items are numbered with upper case letters.
Type "a"	In this type, the list items are numbered with lower case letters.



#### Nested Ordered / Numbered Lists





### Unordered in HTML / HTML Bulleted List

```
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>TypeScript</li>
</ul>
```

<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
    <li>TypeScript</li>
</ul>


https://www.javatpoint.com/html-unordered-list


#### Nested Unordered / Bulleted Lists




### Definition List in HTML / HTML Description List


```
<dl>  
    <dt>Physics</dt>  
    <dd>Physics is study of universe, from smallest subatomic particle to largest galaxies.</dd>  
    <dt>Chemeistry</dt>  
    <dd>Chemistry is study of matter.</dd>  
   <dt>Biology</dt>  
   <dd>Bilogy is study of life.</dd>  
    <dt>Mathematics</dt>  
    <dd>Study of numbers, shapes and patterns is known as Mathematics.</dd>   
</dl>  
```

<dl>  
    <dt>Physics</dt>  
    <dd>Physics is study of universe, from smallest subatomic particle to largest galaxies.</dd>  
    <dt>Chemeistry</dt>  
    <dd>Chemistry is study of matter.</dd>  
   <dt>Biology</dt>  
   <dd>Bilogy is study of life.</dd>  
    <dt>Mathematics</dt>  
    <dd>Study of numbers, shapes and patterns is known as Mathematics.</dd>   
</dl>  

https://www.javatpoint.com/html-description-list

#### Nested Definition / Description Lists





## How to comment unwanted code block in HTML?

Sometime we need to add a block of text or code in our *HTML document* just as an explainer and not as an actual part of code, this process of hiding unwanted code block from the interpreter of the code is known as code commenting. In *HTML* anything in between  `<!--` and `-->` sections is treated as commented code. Here are some examples of comment block in *HTML*.


```
<!-- This is a commented block -->
```

```
<!-- 
Here we just want to explain something intresting
about following section in our HTML code.
-->
```

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
