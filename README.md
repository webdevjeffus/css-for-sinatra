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

```html
/* jeffstrap.css */

/* RESETS */

/* GENERAL STYLES  */

/* COLORS */

/* HEADER STYLES */

/* MAIN STYLES - Yield Block */

/* FOOTER STYLES */

```

### Resets
In this section, we'll add style rules that fix a few things that browsers do to render pages oddly or inconsistently. This includes setting the document to size block elements, including the header, main, footer and any divs, using border-box sizing, instead of the frustrating default, content-box. We'll also set all margins and padding to 0, since different browsers use different defaults for these properties. When we want margins or padding, we'll add them explicitly.

```html
/* RESETS */

html { box-sizing: border-box; }

*, *:before, *:after {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}

```

### General Styles
General styles are styles that will apply throughout the website, in all sections. Here, we'll set our font choices, as well as the colors for everything in the site. Fonts and colors are pretty much the only design decisions we'd have to make to completely change the look of the styling from one project to the next, and by pulling those rules together at the top of the file, we make it easy to make changes.

First, I'll show you the rules for the General Styles section, then we'll talk about it piece by piece:

```html
/* GENERAL STYLES */

/* Fonts */

body { font-family: 'Open Sans', sans-serif; }

h1, h2, h3 {
  font-family: 'Bangers', sans-serif;
}

/* Colors */

html {
  background-color: ;
  color: ;
}

body {
  background-color: ;
  color: ;
}

header, footer {
  background-color: ;
  color: ;
}



```

