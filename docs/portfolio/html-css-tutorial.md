---
title: Introduction to HTML and CSS
---

> **Context**: Onboarding for junior writers.

> **Purpose**: Allow the writers to make minor edits in HTML mode in RoboHelp.

> **Audience**: Juniors with no prior knowledge of HTML and CSS.

## What Are HTML and CSS?

HTML (Hyper Text Markup Language) is a markup language for creating web pages.

CSS (Cascading Style Sheets) is a style sheet language that describes how the HTML elements should look like.

In the modern approach, HTML describes the structure of a page and CSS describes the look and feel of a page. (You can specify the look and feel directly in HTML as well, but that is not recommended nowadays.)

You can think of HTML like the plan of a house. This plan tells you that you have 7 rooms, a terrace etc, but it doesn’t tell you what color the walls are. CSS is the interior designer and painter - it says that the walls in the living room should be white and the walls in the hallway should be green.

---

## HTML Syntax

An HTML document (page) must have a predefined basic structure, otherwise it won't be displayed correctly (or at all).

- It has to start with a `doctype` declaration - this helps the browser to display the web page correctly.
- It needs to continue with a `<html>` tag.
- It needs to have a `<body`> tag after the `<html>` tag (but not necessarily *immediately* after).
- It must end with a closing `</body>` tag and a closing `</html>` tag, in this order.

#### Example - basic HTML document

```html
<!DOCTYPE html>

<html>

<body>

<h1>A heading</h1>

<p>A paragraph.</p>

</body>

</html>
```

The web page displays the contents of the `<body>` tag - this is where you put the documentation. You can see a visual representation [in this page](https://www.w3schools.com/html/html_intro.asp).

---

### HTML Tags and Elements

HTML **tags** are element names surrounded by angle brackets:

```html
<tagname>blah blah</tagname>
```

Most HTML tags come in pairs: a start (opening) tag and an end (closing) tag. Both use the same element name, but the end tag has an extra forward slash. Some elements (for example, the line break - `<br>`) only have a start tag, because do not contain anything. These are called empty elements.

!!!note
    In RoboHelp, the line break tag is always closed (`<br/>`). RoboHelp uses XHTML, which is basically a form of HTML with a stricter structure. For more information about XHTML, see the [W3Schools page](https://www.w3schools.com/html/html_xhtml.asp).

An HTML **element** is everything from the start tag to the end tag:

```html
<tagname>blah blah</tagname>
```

The order of the elements inside the `<body>` tag is mostly up to you. You can nest elements (aka include an element inside another). By default, all the elements in the page contents are nested inside the `<body>` tag.

There are some limitations - some elements cannot be nested inside others (for example, a paragraph cannot contain a heading) - but other than that, the sky is the limit.

!!!note
    Again, as RoboHelp is based on XHTML, the structure is more strict than pure HTML. In practice, you probably won't have to worry about this, because RoboHelp does most of the work for you. (When you use the Author mode, RoboHelp inserts the tags correctly in the first place; when you use HTML mode, RoboHelp tries to correct them for you, if needed.)

---

### HTML Attributes

Attributes provide additional information about the element they are associated to - for example, the style of a paragraph. Attributes are always specified in the start tag and have the format `attribute_name="attribute_value"`. 

In a tag, this will look like: 
```html
<tag attribute_name:"attribute_value">blah</tag>
```

For example:

```html
<p style="color:red">This is a red paragraph</p>
```

---

## Commonly-Used HTML Tags

This page describes the HTML tags that we generally use in RoboHelp. Only the most basic information is included - for anything else, see the [W3Schools HTML 
tutorial](https://www.w3schools.com/html/default.asp). You can test HTML yourself using the [editor](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default) on the W3Schools website.

---

### Paragraph and Line Break - `<p>` and `<br>`

The`<p>` tag defines a paragraph, which can contain as many sentences as you want.

You can manually force part of a paragraph tag to start on the next line by using a line break tag (`<br/>`), but this is not recommended in our documentation. If you need a new paragraph, you should actually **use** a new paragraph.

To insert a new paragraph in a visual editor like Word, you press Enter. To insert a new line break in a visual editor like Word, you press Shift+Enter. In general, the distance between two paragraphs is larger than the distance between two lines in the same paragraph.

#### Example - simple paragraph

**HTML:**

```html
<p>This is a paragraph.</p>
```

**Output:**

This is a paragraph.

#### Example - paragraph with line break

**HTML:**

```html
<p>This is the first sentence of a paragraph. <br/> Because I added a line break manually, the second sentence will appear on the next row.</p>
```

**Output:**

This is the first sentence of a paragraph.  
Because I added a line break manually, the second sentence will appear on the next row.

#### Example - two paragraphs

**HTML:**

```html
<p>This is the first paragraph.</p>

<p>This is the second paragraph.</p>
```
**Output:**

This is the first paragraph.

This is the second paragraph.

---

### Headings - `<h1>`, `<h2>` etc

Six levels of headings can be used in HTML - from `<h1>` to `<h6>`. Heading 1 is the most important and, by default (without any CSS) it is displayed in the largest font.

#### Example - heading

**HTML:**

```html
<h2>My heading 2</h2>
```

**Output:**

<h2>My Heading 2</h2>

---

### Division (section) - `<div>`

The `<div>` tag defines a section (or *div*ision) of a page. A section can contain one or more paragraph and headings. It can be useful when you want to use CSS to set the same formatting for all the elements inside.

#### Example - div

```html
<div>

<h1>The heading of this section</h1>

<p>This section contains one paragraph.</p>

</div>
```

---

### Span - `<span>`

The `<span>` tag is generally used to group a part of a paragraph so that formatting can be applied through CSS. A `<span>` by itself doesn't do anything.

!!!note
    In RoboHelp, if you are fixing formatting by running search & replace, you may end up with a lot of empty <span> tags. They won't break anything and won't be visible in the output, but it's better to delete them to have cleaner, easier-to-follow code.

---

### Lists - `<ol>`, `<ul>`, `<li>`

There are two types of lists:

- Ordered list (aka "numbered") - `<ol>`
- Unordered list (aka "bulleted") - `<ul>`

Each step or item in a list is enclosed in `<li>` and `</li>` tags.

The "ordered" and "unordered" terms are the "most" correct - an ordered list is a list where the items follow a certain sequence, but it does not have to be numbered, it can also use *a, b, c* or *i, ii, iii*. The same goes for the unordered lists - they can use bullets, circles, arrows etc.

By default, an ordered list is numbered starting with 1 and an unordered list has bullets. This default can be changed through CSS.

#### Example - basic ordered list

**HTML:**
```html
<ol>

  <li>This is the first step.</li>

  <li>This is the second step.</li>

</ol>
```

**Output:**

1. This is the first step.
2. This is the second step.

#### Example - basic unordered list

**HTML:**
```html
<ul>

  <li>This is an item.</li>

  <li>This is another item. /</li>

</ul>
```

**Output:**

- This is an item.
- This is another item.

You can force an ordered list to start from a number different from 1, **but you should never do it** in our documentation.

#### Example - ordered list with forced start

**HTML:**
```html
<ol start="50">

  <li>This is step number 50.</li>

  <li>This is step number 51.</li>

  <li>Step number 52, but you should not do this.</li>

</ol>
```

**Output:**

50. This is step number 50.

51. This is step number 51.

52. Step number 52, but you should not do this.

!!!note
    As you can see, you do not need paragraph tags for an HTML list to be valid. **However, in RoboHelp is it best practice to add them.** (The paragraph tag is represented in RoboHelp by the style Normal.) If the CSS definition of the paragraph style is different from the definition of the list items, then a list with Normal style will look different from a list without Normal style.

#### Example - ordered and unordered lists with added paragraphs
```html
<ol>

  <li><p>This is the first step.</p></li>

  <li><p>This is the second step.</p></li>

</ol>

<ul>

  <li><p>This is an item.</p></li>

  <li><p>This is another item.</p></li>

</ul>
```

An item (`<li>`) can contain one or more paragraphs. This is commonly used in RoboHelp to ensure proper alignment for procedures.

#### Example - list item with several paragraphs
```html
<ol>

  <li><p>This is the first paragraph in the first step.</p>

  <p>This is the second paragraph in the first step.</p></li>

  <li><p>This is the second step.</p></li>

</ul>
```

---

### Image - `<img>`

The `<img>` tag is used to embed an image. You always need to use two attributes: `src` (the source of the image, its location) and `alt` (the description, displayed in case the image cannot be loaded).

#### Example - img

```html
<img src = "cat.png" alt="This is a cat image"/>
```

By default, the image is displayed in full size. If you want, you can use the `width` and `height` attributes to resize it. You should avoid this. (On the other hand, RoboHelp automatically inserts the height and weight attributes and sets them to the full size of the image. Don't be surprised if you see them.)

---

### Address (link) - `<a>`

The `<a>` tag is used to add a hyperlink. You always need to use the href attribute to specify the actual address.

#### Example - link

```html
<a href = "www.example.com">
```

You can specify how to open the linked page using the `target` attribute. For example, to open the page in a new tab you would use `_blank`.

#### Example - link opened in new tab

```html
<a href= "www.example.com" target = "_blank">
```

---

### Table - `<table>`, `<th>`, `<td>`, `<tr>`

The `<table>` tag is used to define a table, but several more tags are needed to have an actual table with rows and columns:

- `<tr>` - defines the regular table rows.
- `<th>` - defines the table header cells. **Note**: RoboHelp doesn't use
                this when you insert a new table.
- `<td>` - defines the regular table cells.

#### Example - table

**HTML:**
```html
<table>

  <th>

    <td>Header row, first column</td>

    <td>Header row, second column</td>

  </th>

  <tr>

    <td>Second row, first column</td>

    <td>Second row, second column</td>

  </tr>

<table>
```
**Output:**

| Header row, first column | Header row, second column |
| --- | --- |
| Second row, first column | Second row, second column |

You can use the `colspan` and `rowspan` attributes for the `td` and `th` tags to create merged rows or columns (but it's easier to just do it from the RoboHelp Author view).

---

## CSS Syntax

CSS (Cascading Style Sheets) is used to indicate what HTML elements should look like. You can define the CSS inside an HTML document or use an external CSS file.

---

### Syntax

To associate a CSS style to an HTML tag, you need to use a syntax based on a selector and a declaration. The selector is the first part - the tag or class you want to apply the style to. The declaration is the style itself.

The basic syntax is:

`tag {`

`property:value;`

`property:value;`

`}`

Note that there is a semicolon after the last line as well.

With this syntax, all the text that is inside the specified tag will be styled as indicated. In this example, all paragraphs will be in 18pt Arial font.

#### Example - CSS styling

```css

p {

font-family: Arial;

font-size: 18pt;

}
```
You can also use a more complicated selector, for example to say that you want paragraphs that use the class Body to have one style and paragraphs that use the class Note to have a different style.

#### Example - CSS classes

```css
p.Body {

  font-family: Arial;

  font-size: 18pt;

}

p.Note {

  font-family: Verdana;

  font-size: 18pt;

}
```

For other types of selectors, see the [CSS Syntax and Selectors](https://www.w3schools.com/css/css_syntax.asp) page on W3Schools.

---

### Internal Style Sheet

To define an internal style sheet, you enter the CSS in the `<head>` of the page, inside the `<style>` tag.

#### Example - internal stylesheet
```html

<head>
  <style>
    p {
       font-size: 10pt;
    }
  </style>
</head>
```

---

### External Style Sheet

To define an external style sheet, you enter the location of the CSS file in the `<head>` of
            the page, inside the `<link>` tag.

#### Example - external stylesheet
```html
<head>

  <link rel="stylesheet" type="text/css" href="help_styles.css">
  
</head>
```