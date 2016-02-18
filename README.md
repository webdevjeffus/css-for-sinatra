# CSS for Sinatra
#### How to develop a flexible CSS stylesheet to instantly style any Sinatra-based web app
(Repo and accompanying talk currently under development)

## What this How-to is, and what it isn't
The purpose of this how-to is to help you develop a flexible CSS stylesheet that can be dropped onto any Sinatra-based web app, instantly applying "good-enough" styling to the entire website. This is accomplished using type-based CSS selectors CSS along with basic semantic HTML practices. When properly executed, the final stylesheet should work on _any_ Sinatra web app, as well as most Rails-based apps, without adding any classes to the HTML, or any additional styling rules to the CSS file.

However, this strategy is _not_ appropriate for styling anything beyond the MVP stage. Type-based selectors are measurably less performant than class-based or id-based selectors. While this isn't likely to noticeably affect performance on a locally-hosted app in development, it could have a perceivable impact on the performance of a large, publicly-deployed app.

### Using this How-to
This README file contains the main flow of the how-to; read or skim it from top to bottom to follow the process it describes. It will also contain links to other files in the repo, or to outside resources, when appropriate.

## Setting up your HTML file
For this to work, you need to set up your layout.erb (or layout.html.erb, in Rails) following some basic best-practices for semantic HTML. We'll be relying on HTML semantics to differentiate between tags of the same type in different areas of the page. For example, our stylesheet will handle an **\<h1>** tag differently if it appears in the app's **\<header>** element than if it is in the **\<main>**.

When you set up your layout.erb, include the following code in the document's **\<body>** element:
```html
<body>
  <header>
    <h1><a href="/">APP NAME</a></h1>
    <nav>
      <ul>
        <li><a href="#">LINK 1</a></li>
        <li><a href="#">LINK 2</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <%= yield %>
  </main>

  <footer>
    <p>FOOTER TEXT</p>
  </footer>
</body>
```

The "yield," wrapped in a printing erb tag, will tell Sinatra (or Rails) to render each view inside the **\<main>** element; you don't need to do anything else to this section. The **\<footer>** tag is even less complex&mdash;just drop your own footer text into the **\<p>** element.

The **\<header>** element has a bit more going on, but it's easy enough to understand. First is the **\<h1>** element that will serve as the logo for your MVP version. The **\<a>** link to the root route will make the logo a link back to the home page from any place in your app, so be sure you've defined the root among the routes in your controllers.

We'll set up the header so that the logo displays at the left edge, and the nav links appear lined up on the right. To make this happen, we'll put the links inside list items (**\<li>**), which are included in a single unordered list (**\<ul>**). The list is held within a semantic **\<nav>** element, so we can select it easily for styling.

We'll loop back around and cover the **\<head>** element in your layout.erb file after we've talked about the contents of the CSS file.

## Creating the CSS file
In this tutorial, we'll actually be employing _three_ CSS files: normalize.css (more on this in the **\<head>** section, below); application.css (where you'll put any specific styling for this particular app; again, more in the **\<head>** session); and this type-based stylesheet, which you can name anything you like, so long as it's distinctive. My version was named "jeffstrap.css" by my DBC classmate, Jon Chen, so I'll use that name for the rest of this how-to.

I'll begin by creating my **jeffstrap.css** file, and setting up some commented-out section heads to help organize it. Here's how it looks at first:

```css
/* jeffstrap.css */

/* RESETS */

/* DESIGN STYLES  */

/* HEADER STYLES */

/* MAIN STYLES - Yield Block */

/* FOOTER STYLES */

```

### Resets
In this section, we'll add style rules that fix a few things that browsers do to render pages oddly or inconsistently. This includes setting the document to size block elements, including the header, main, footer and any divs, using **border-box** sizing, instead of the frustrating default, content-box. We'll also set all **margins** and **padding** to 0, since different browsers use different defaults for these properties. When we want margins or padding, we'll add them explicitly.

```css
/* RESETS */

html { box-sizing: border-box; }

*, *:before, *:after {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}

body {
  margin: 0 auto;
  min-width: 600px;
  max-width: 800px;
}

h1, h2, h3, h4, h5, h6, p {
  margin-bottom: 0.5rem;
}

```

In the **body** rule, we first set the left and right **margin** to **auto**, so that the app content will be centered on the page. We'll also set **min-width** and **max-width** values to keep our page within a range in which our styling works. If you want a fully-responsive site, you'll need to add media queries&mdash;but that's a separate talk.

Finally, in the "Resets" section, we'll add a bottom margin of 0.5rem, to put just a bit of vertical whitespace between elements on the page. (I'll talk more about rems in section on header styles, below.)


### Design Styles
Design styles apply throughout the website, across all pages and in all sections; they determine the look of the app. Here, we'll set our font choices, as well as the colors for everything in the site. Fonts and colors are all we need to change to completely alter the styling from one project to the next. By pulling those rules together at the top of the file, we make it easy to make changes.

```css
/* DESIGN STYLES */

/* Fonts */

html { font-family: 'Open Sans', sans-serif; }

h1, h2, h3, h4 {
  font-family: 'Bangers', sans-serif;
}
```

#### Fonts
In the Fonts section, we'll be using two fonts&mdash;one display font and one body font&mdash;and we have only need two rules to assign them throughout the app. The first, declared for the **\<html>** element, assigns our body font (Open Sans, in this case) as the default font for the site. The only elements that will use the display font we've chosen (Bangers, for this app) are the larger heading elements: **\<h1>** down through **\<h4>**. Chances are we won't even need any headings smaller than **\<h4>**, and if we do, we'll just style them as bold-face Open Sans.

We'll be using Google Fonts to serve our chosen fonts; we set that up in the **\<head>** element, which I'll cover below.

```css

/* Colors */

html {
  background-color: #888;   /* medium gray */
  color: #ddd;              /* dark gray */
}

body,
header nav input[type="submit"] {
  background-color: #222;    /* dark gray */
}

main {
  background-color: #ddd;   /* light gray */
  color: #222;              /* dark gray */
}
```

#### Colors
We're using a deliberately simple color scheme here&mdash;four shades of gray for our backgrounds and font colors, and a contrasting red for links which stands out well against both light and dark shades of gray. Even so, there are several CSS rules that need to include color properties, so this section _looks_ kind of long. Thats because I've moved _all_ the rules which specify color to this section, for ease of maintenance. This probably isn't how you'd set up a stylesheet for the production version, but it makes this quick-and-dirty stylesheet easier to change.

The **html** and **body** rules set up the background-color and font color for the site. The **html** background is a medium shade that contrasts with both the dark color of the **header** and **footer**, and the light shade in the **main** element. I've added the **header nav input[type="submit"]** selector to the **body** rule, since we will want the background color for the "Logout" button on our nav bar to match the surrounding header. More on the "Logout" button in the "Header Styles" section, below.

```css
a:link,
a:visited,
header nav input[type="submit"] {
  color: #c00;              /* darker contrasting color; inactive links */
  font-weight: bold;
  text-decoration: none;
}

a:hover,
a:active,
header nav input[type="submit"]:hover,
header nav input[type="submit"]:active {
  color: #f22;              /* brighter contrasting color; active links */
}

header nav li {
  border-left: 3px solid #888;    /* match html background-color */
}

main table,
main th,
main td {
  border: 1px solid #222;         /* match body background-color */
  background-color: #eee;         /* lighter than main background-color */
}

```



The **main** rule sets the light background and dark font color for everything except the **header** and **footer** of the app.

The next two rules, which start with **a:link** and **a:hover**, style the links throughout the page. The first rule, for inactive links, assigns a rich, dark red which contrasts nicely with both the light and dark backgrounds in the site. This rule also sets link text to bold, with no underline; these properties are inherited by active links as well. The second link rule highlights active links a vivid red, which is noticeably brighter than the dark red of the inactive links. The other selectors in these rules, which include **input[type="submit"]**, style the "Logout" button to use the same colors as other links.

Since the logo **\<h1>** in the header is also a link, it will automatically be styled with the contrasting color, helping it grab the user's attention. If red is too bold a choice for the contrasting color, try two shades of turquoise, gold, blue, green, or even pink. Just be sure that both shades contrast well with both the light and dark shades in your background. Readability trumps aesthetics for an MVP.


### Header Styles
In this section, we'll set up the rules for the elements in our **\<header>** section. It is here that we see the flexibility of our type-based CSS selectors coming into play.

```css
header { padding: 1rem; }

header h1 {
  font-size: 3rem;
  float: left;
}

header nav ul {
  list-style: none;
  float: right;
}

header nav li {
  display: inline-block;
  padding-left: 1rem;
  border-left: 3px solid #888;        /* medium gray */
}

header nav li:first-child { border-left: 0;}

header nav input[type="submit"] {
  overflow: visible;
  text-align: center;
  font-weight: bold;
  width: auto;
  border: none;
  background-color: #222;           /* dark gray */
  color: #c00;                      /* darker contrasting color */
}

header nav input[type="submit"]:hover,
header nav input[type="submit"]:active {
  color: #f22;                      /* brighter contrasting color */
}
```

#### header
The general **header** rule just puts 1rem of padding around all four sides of the header, to keep the text from banging against the edges. I prefer rems ("root ems") for most CSS measurements because they scale most smoothly if you make the site responsive. By default, 1rem = 16 pixels; if you want to change that value, just set a **font-size** property to some value other than 16 pixels in the style for the root element, **\<html>**. If you later change the root em value, then every font-size or element dimension that is sized in rems will scale with the root em.

#### header h1
In the **header h1** rule, we specify any and all **\<h1>** elements in the **\<header>** should be rendered at a size of 3rems. Our CSS selector for the rule, "header h1", won't touch **\<h1>**s in any other element of the app&mdash;for example, an **\<h1>** in the **\<main>** element will be displayed at its default size, not at the 3rems specified in this rule. We also set the **header h1** to **float: left**, so that it displays against the left margin of the header section.

#### header nav
The rest of the styles in the Header Styles section deal with the components of the header's **\<nav>** element. To be sure that we don't affect list or nav elements elsewhere in the app, we begin each selector with a mention of both the **\<header>** and the **\<nav>** types. The **header nav ul** rule eliminates the bullet points that appear by default on unordered lists.

The **header nav li** rule changes the links in the nav bar to appear on a horizontal line, instead of as a vertical list; the "border-left" property establishes a vertical border between each pair of links. The **header nav li:first-child** rule _turns off_ that border to the left of the first link on the nav bar.

#### header nav input[type="submit"]
The remaining rules, which deal with **header nav input[type="submit"]** elements, are necessary because HTML and Sinatra all but _require_ the "logout" link to be created as a button. Since ending the current session requires a POST route with a hidden "delete" object, it is usually created as a single-button form, rather than as a simple link. The last few rules in the Header Styles section are necessary to remove the border and background from the button, so that it looks and behaves like the other links in the nav bar.

**Color Note:** If you change the colors for the app in the Design Styles section, be sure to make the corresponding changes to the links here in the header nav bar. For this reason, it's a good idea to include my comments pointing out color properties in your CSS file.

**Performance Note:** Selectors like this one, that depend on an attribute of an html tag, are just about the slowest possible CSS selector. We use them here because we are deliberately trying _not_ to have to add classes to our html file; in the deployed version of the app, you'd be better off to use a class as a selector, even if it's unique.

### Main Styles
In **main** styles section, we see our type-based selectors really pay off: they let us write an unlimited number of pages, partials, and forms which look decent (not great, but decent) with absolutely zero time spent on styling. Again, I'll show you the basic CSS code first, then break it down rule by rule.

```css
main {
  padding: 1rem;
  clear: both;
}

main ul { margin: 0 auto 1.5rem; }

main ul li {
  list-style: none;
  margin: 0.5rem 0 1rem;
  font-size: 1.25rem;
}

main input {
  display: block;
  margin-bottom: 0.75rem;
  width: 100%;
}

main input[type="text"] { padding: 0 0.5rem; }

main textarea {
  width: 100%;
  height: 5rem;
  margin-bottom: 1rem;
  padding: 0.5rem;
}

main input[type="submit"] { width: 12rem; }
```

#### main
The padding in the **main** rule just pushes the text in from the edges of the **main** element a little bit; we use rems instead of pixels here so that the width of the padding scales automatically. The **clear: both** property is necessary to clear out the floats we used in the header; again, this isn't how we'd do it in the production version of our app, but it works fine for this quick-and-dirty stylesheet.

Headers, paragraphs and links in the **main** element use the defaults set in the Resets and Design Styles, so we generally don't need to add anything for them here. If you decide to increase the size of your headers from the default&mdash;and you very well might&mdash;do that in the Resets section of this stylesheet.

#### main ul and main ul li
Almost every index view in a Sinatra or Rails app is going to display some sort of list of objects in the data base, but the default presentation of unordered lists is very 1994. To bring our stylesheet into the current century, we'll set **list-style** to **none**, to eliminate the bullets. The **margin** properties are necessary to get the list back into the right spot once we've taken out the bullets. I like to set the font-size for list items to 1.25rem, because these lists tend to be pretty important on our pages, and we want them to be prominent.

#### Forms: main input and main textarea
Forms are also very common in Sinatra and Rails apps, but again, the default styling for them is generally pretty ugly and unfriendly. We'll handle this mostly by styling the form input elements.

First, we'll set the **display** property of **main input** elements to **block**, so each one appears on its own line on the page. If you add a label for an input, it will default to **display: inline**, so labels will show up to the left of the text box they identify. If you prefer labels on the line above, you can add the following style rule to your CSS file:

```css
main label { display: inline; }
```

The margins and padding we set for **input** and **textarea** elements add a bit of whitespace to the form, while setting the **width** of each to **100%** makes each field wide enough to display most input data without scrolling. Remember that **\<input>** elements can only display a single line of text. For this reason, you'll want to use a **\<textarea>** form element for any field where users will be entering more than a few words, such as blog posts or comments.

The **main input[type="submit"]** rule sets **Submit** buttons to 12rem, which is wide enough to display the full text on the button in most cases. If you need more space, just up the rem in this rule.

### Tables
If your app displays complex user or game data, you'll probably want to show it in a table. Here are the rules to add to your stylesheet to automatically style a simple table:

```css
main table {
  margin-bottom: 1rem;
  border: 1px #222 solid;
  background-color: #fff;
  width: 100%;
}

main tr:first-child {
  background-color: #aaa;
  color: #222;
}

main th,
main td {
  padding: 0.25rem 0;
  width: 25%;
  text-align: center;
  border: 1px solid #222;
}
```

#### main table
In this rule, we
