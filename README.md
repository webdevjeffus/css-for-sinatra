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

/* COLORS */

/* HEADER STYLES */

/* MAIN STYLES - Yield Block */

/* FOOTER STYLES */

```

### Resets
In this section, we'll add style rules that fix a few things that browsers do to render pages oddly or inconsistently. This includes setting the document to size block elements, including the header, main, footer and any divs, using border-box sizing, instead of the frustrating default, content-box. We'll also set all margins and padding to 0, since different browsers use different defaults for these properties. When we want margins or padding, we'll add them explicitly.

```css
/* RESETS */

html { box-sizing: border-box; }

*, *:before, *:after {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}

```

### Design Styles
Design styles apply throughout the website, across all pages and in all sections; they determine the look of the app. Here, we'll set our font choices, as well as the colors for everything in the site. Fonts and colors are all we need to change to completely alter the styling from one project to the next. By pulling those rules together at the top of the file, we make it easy to make changes.

```css
/* DESIGN STYLES */

/* Fonts */

html { font-family: 'Open Sans', sans-serif; }

h1, h2, h3, h4 {
  font-family: 'Bangers', sans-serif;
}

/* Colors */

html {
  background-color: #888;   /* medium gray */
  color: #222;              /* dark gray */
}

body { background-color: #ddd; }    /* light gray */
form { background-color: #bbb; }    /* light medium gray */

header, footer {
  background-color: #222;   /* dark gray */
  color: #ddd;              /* light gray */
}

a:link,
a:visited {
  color: #c00;           /* darker contrasting color for inactive links */
  font-weight: bold;
  text-decoration: none;
}

a:hover,
a:active {
  color: #f22;          /* brighter contrasting color for active links */
  font-weight: bold;
  text-decoration: none;
}

```

#### Fonts
In the Fonts section, we'll be using two fonts&mdash;one display font and one body font&mdash;and we have only need two rules to assign them throughout the app. The first, declared for the **\<html>** element, assigns our body font (Open Sans, in this case) as the default font for the site. The only elements that will use the display font we've chosen (Bangers, for this app) are the larger heading elements: **\<h1>** down through **\<h4>**. Chances are we won't even need any headings smaller than **\<h4>**, and if we do, we'll just style them as bold-face Open Sans.

We'll be using Google Fonts to serve our chosen fonts; we set that up in the **\<head>** element, which I'll cover below.

#### Colors
We're using a deliberately simple color scheme here&mdash;four shades of gray for our backgrounds and font colors, and a contrasting red for links, chosen because it stands out well against both light and dark shades of gray. Since the logo **\<h1>** in the header is also a link, it will automatically be styled with the contrasting color, helping it grab the user's attention.

If red is too bold a choice for the contrasting color, try two shades of turquoise, gold, blue, green, or even pink. Just be sure that both shades contrast well with both the light and dark shades in your background. Readability trumps aesthetics for an MVP.

Note also that we set the font-weight and text-decoration properties for links (**\<a>** tags) here. This means that all links will appear in bold-face type, but without underlines, for a cleaner, more modern look than the default, underlined link style.

### Header Styles
In this section, we'll set up the rules for the elements in our **\<header>** section. It is here that we see the flexibility of our type-based CSS selectors coming into play.

```css
header { padding: 1rem; }

header h1 { font-size: 3rem; }

header nav ul { list-style: none; }

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

The first rule just puts 1rem of padding around all four sides of the header, to keep the text from banging against the edges. I prefer rems ("root ems")for most CSS measurements because they scale most smoothly if you make the site responsive. By default, 1rem = 16 pixels; if you want to change that value, just set a font-size property to a value other than 16 pixels in the style for the root element, **\<html>**. If you later change the root em value, then every font-size or element dimension that is sized in rems will scale with the root em.

In the next rule, we specify any and all **\<h1>** elements in the **\<header>** should be rendered at a size of 3rems. Our CSS selector for the rule, "header h1", won't touch **\<h1>**s in any other element of the app&mdash;for example, an **\<h1>** in the **\<main>** element will be displayed at its default size, not at the 3rems specified in this rule.

The rest of the styles in the Header Styles section deal with the components of the header's **\<nav>** element. To be sure that we don't affect list or nav elements elsewhere in the app, we begin each selector with a mention of both the **\<header>** and the **\<nav>** types. The "header nav ul" rule eliminates the bullet points that appear by default on unordered lists. The "header nav li" rule changes the links in the nav bar to appear on a horizontal line, instead of as a vertical list; the "border-left" property establishes a vertical border between each pair of links. The "header nav li:first-child" rule _turns off_ that border to the left of the first link on the nav bar.

The remaining rules, which deal with "header nav input[type="submit"]" elements, are necessary because HTML and Sinatra all but require the "logout" link to be created as a button. Since ending the current session requires a POST route with a hidden "delete" object, it must be created as a single-button form, rather than as a simple link. The last few rules in the Header Styles section are necessary to remove the border and background from the button, so that it looks and behaves like the other links in the nav bar.

If you change the colors for the app in the Design Styles section, be sure to make the corresponding changes to the links here in the header nav bar. For this reason, it's a good idea to include my comments pointing out color properties in your CSS file.