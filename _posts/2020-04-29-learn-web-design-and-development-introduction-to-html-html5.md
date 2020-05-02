---
layout: post
title:  "Learn web design and development | Introduction to HTML / HTML5"
author: sma
categories: [ WEB, WEBDESIGN, HTML, HTML5, CSS, CSS3 ]
image: assets/images/posts/campaign-creators-iEiUITs149M-unsplash.jpg
imageAttribution: Photo by Campaign Creators on Unsplash
description: "In this article we will learn about HTML and HTML5, we will learn topics like what html is, basic structure of html document, understanding html semantic elements, usage of paragraphs, heading, hyperlinks, understanding ordered and unordered lists in html, adding images in html page and getting user input using html forms."
tags: [featured]
---

## What is HTML 

**H**yper **T**ext **M**arkup **L**anguage *a.k.a* **HTML** is a markup language used to define the structure of web content (web pages). HTML is not a programming language in other words HTML can not be used build any kind of logic. As explained earlier HTML is used to define the structure of web content or web pages, this structure is defined with the help of elements or tags like *paragraph*, *heading*, *list*, *link* or *image*.  These HTML tags are written in format of plain text file which is then rendered by a *web browser (software application program used to display web pages or web content)*. **HTML 5** is the current version  of HTML, *HTML5* was released in 2014 which replaced a very long lived (almost 14 years) HTML4.


## Understanding Basic HTML Document

Let's dive deep and understand the HTML with some practice examples, before we explore other powerful areas of HTML let's first understand what HTML document is.

1. Create a new folder *(in location of your choosing, like desktop...)* and call it `learning-html`.
2. Create a new file named `hello.html` in folder `learning-html`.
3. Open the newly created file `hello.html` in a text editor like `Visual Studio Code`.


>> **NOTE** You can install Visual Studio Code extension like *Live Server* for quick and easy verification of our html code in local environment. Please check following official docs to search, install and use extensions in Visual Studio code.
[Extension Marketplace / Increase the power of Visual Studio Code through Extensions](https://code.visualstudio.com/docs/editor/extension-gallery){:target="_blank"}.


![Visual Studio Code extension Live Server]({{ site.baseurl }}/assets/images/posts/vs-code-extensions-live-server.jpg)


Copy following contents in `hello.html`, don't worry if you can't understand it, I will explain each part of the code and structure in detail.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Web Page</title>
</head>
<body>
    <h1>Hello WWW!</h1>
</body>
</html>
```

Save the document, and click on **Go Live** button *(this will only be available if you have installed a live server extension in visual studio code as shown in image above)*. Clicking the **Go Live** button will open current html file `hello.html` in default browser of you computer as shown below.

![My First Web Page]({{ site.baseurl }}/assets/images/posts/hello-html.jpg)


**Congrats**, you have created  your first web page, now let's explore the  ingredients of our first web page *`(hello.html)`*.


### <!DOCTYPE html>

Web browser *(Chrome, Firefox, Safari, Edge)* is used to render different type of contents including HTML web pages, images, audio and video. To check this simply launch any web browser on you computer and open a picture file *(like jpg or png)*  this is normally done using File -> Open menu and the browser will show this picture inside it's main content area. This procedure can also be used to open regular text files and we know *HTML* pages are also plain text file so how the browser will treat `.html` files differently? Here comes the `<!DOCTYPE html>` declaration, this declaration tells the browser to treat upcoming text content as *HTML web page*. `!DOCTYPE` declaration also informs the browser about specific *HTML standard* used in the *HTML document* and `<!DOCTYPE html>` is specific to *HTML 5*.

### Understanding HTML Tags or elements

*HTML tags* are the elements used to structure and compose *HTML pages*. HTML tags are enclosed in `<>` (angle brackets) like `<b>`, there are two types of html tags i,e. with and without contents, the one with content begin with `<>` and end with `</>` e,g. `<b> This is bold </b>`. Let's look at common syntax of  a html tag.


#### Regular HTML Tags
```
<tagname> contents ...  </tagname>
```

In above example the `tagname` tag starts with `<tagname>` and ends with `</tagname>` with content in between.

##### Examples of Regular HTML Tags

`<h1> This is Heading 1 </h1>`

`<h2> This is Heading 2 </h2>`

`<b> This is bold text </b>`


#### Self Closing HTML Tags
```
<tagname />
```

In above example the `tagname` tag is a self closing html tag where `/>` denotes in-place closing of this tag.

##### Examples of Regular HTML Tags

`<br />`  is used to add a line break.

`<img />` is used to add an image in web page.

`<hr />` is used to add a horizontal rule in webpage.


#### Understanding Attributes in HTML tags

Attributes are used to define additional properties of *HTML elements or tags*. Attributes are normally defined as `name="value"` pair.


##### Examples of HTML Tags with attributes

Following example uses  `src` attribute to set *image source* of `img` tag.

```
<img src="image1.jpg" />
```

Now we have some understanding the key constituent i,e. *HTML Tag* so let's explore the structure of our first html page mentioned above.


## <html> </html> tag

`html` tag defines the HTML document, so anything that we want to represent or associate with our *html web page* needs to be enclosed in `<html>...</html>` tag.

## Head section of the html document: <head> </head>

HTML document is primary divided in two main parts `head` and `body`, for simplicity you can say the head contains the information, references and hints for browser and body contains the information that is rendered by browser for user presentation.

Some of the elements enclosed in `<head>...</head>` section  of *HTML* document are as following.

### title: <title> </title>
`title` tag is used to define the title of `HTML web page`. Title of the web page is displayed in the title bar of the browser.

### meta: <meta> </meta>
`meta` tag is used to define metadata *(information about data)* of the web page. Specific metadata of the webpage like `keywords`, `viewpoint`, `charset`, `description` are defined using attribute *(key / value pair)* of `<meta>` tag. In the sample web page `hello.html`, we have declared `charset` and `viewpoint` metadata as following.

`<meta charset="UTF-8">` : *charset* metadata is used to define the character-set encoding of the webpage, here we have defined `UTF-8` *(8-bit Unicode Transformation Format)* as the character-set encoding of our webpage.


`<meta name="viewport" content="width=device-width, initial-scale=1.0">` : `name="viewport"` metadata is used to control dimensions of visible area of the web page. Visible area of web page can vary device to device *(smaller for mobile and larger for computer screen)*. Using `width=device-width` value for the `content` attribute we are guiding the target browser to respect the screen width of the device, so browser on a *mobile device* and *computer* will render the webpage according to width of respective device. The second value `initial-scale=1.0` for the `content` attribute is used to set initial *zoom level* when the page is loaded.


## Body section of the html document: <body> </body>

The body tag contains the contents of *HTML document*, the content may include text, images, audio and video.

**Example  body section**

```
<body>
    <h1>Hello Web!</h1>
    <p>
        This is a paragraph
    </p>
    <img src="/images/image1.jpg" alt="a beatutiful image" />
</body>
```


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

---
This article is part of [Web Design and Development]({{ site.baseurl }}{% post_url 2020-05-02-learning-web-design-and-development-series %}) series.
