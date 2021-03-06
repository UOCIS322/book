# Doing it in Style with CSS

So far we haven't said anything about how to control colors, typefaces, page layout, etc.  In the early days of the web, these were controlled directly in HTML, with tags like &lt;center&gt; and even the notorious &lt;blink&gt;.   Page layouts were often produced by creating monstrous tables with cells for each section of the page.  Worse, to obtain visual effects that were beyond the power of HTML, many authors typeset fancy headings into images and placed those images in web pages in place of text.  You may imagine how well that worked for blind users who depend on screen readers.   We don't do that anymore.  We use style sheets coded in the _Cascading Style Sheet_ \(CSS\) notation instead.

It may seem annoying at first that we use two completely different notations for the content of a web page \(HTML\) and for styling \(CSS\).  Yet another language to learn!  Yet there are good reasons for this separation.

### Separation of Concerns: Content vs Presentation

Our biggest challenges in constructing software of any kind, from web sites to embedded systems to enterprise applications, is controlling complexity.  We can't fit all the details of a software system in our head at once, let alone think clearly about all the possible ways we could solve every problem at once.   Moreover, we can't solve problems once and for all and leave them be, because every successful software system continues to evolve and adapt to new needs.  If we build a successful system, we can look forward to revisiting it and getting to know it again and again, and those details that were hard enough to keep straight when we initially built it will be twice as hard to absorb when we have been away from the project for weeks or months or years.  We must do everything we can to break the complexity down into subproblems that we can understand and solve separately.

One of the useful separations that we can \(mostly\) make is between the content and logical structure of the information to be displayed, on the one hand, and presentation issues like layout, typefaces, and use of color on the other.  This is a particularly useful separation because the _content_ of a web site or application tends to evolve somewhat separately from its \_presentation:  \_Often we will revise one \(e.g., adding some content\) without modifying the other.  Identifying concerns that tend to change independently is a key technique in software engineering, and the basic architecture of the world-wide web is designed to support  separation between concerns of content and presentation.   We write content in HTML and control presentation with CSS, in style sheets.  By localizing presentation decisions in a style sheet, we can write a single style sheet that applies to several pages in a web site.  This helps us maintain consistency across the site, and  greatly reduces the work and chance of errors when we make a style change that should apply to all pages.   Two years from now, when everyone is reading web pages on larger phones, or smaller smart watches, or some device we haven't yet imagined, new style sheets can be designed to optimize presentation for those devices with a minimum of disruption to the content.

### Loading a Style Sheet \(or several\)

Style specifications appear in documents called _style sheets_, which are usually separate from the web pages they are applied to, so that a single style sheet can be applied to several pages.  A link in the &lt;head&gt; section of a web page can tell the browser to obtain and apply a style sheet:

```
<link href="./styles/style.css" rel="stylesheet" type="text/css" />
```

A single web page may link multiple style sheets, and may indicate that the style to be applied depends on the device on which the web page is displayed.  The web client \(browser\) will then request just the appropriate stylesheets from the web server.  For example, we may wish to use a lot of color on the screen, but minimize areas of dark color when printing. We might even display a navigation sidebar on the screen, but suppress it when printing a page.  It is common to use one style sheet to control presentation on a screen, and another to control presentation in print media:

```
   <link rel="stylesheet" type="text/css" href="./lib/cis322.css" media="screen"/>
   <link rel="stylesheet" type="text/css" href="./lib/cis322-print.css" media="print"/>
```

### Associating styles with content

Style specifications in a CSS style sheet are associated with elements in the HTML by matching.  We can match by element type \(e.g., subheads with tag &lt;h2&gt;\), or with named "classes" , or with identifiers attached to individual elements.

Suppose we wanted all the first level headings  \(&lt;h1&gt; elements\) to be red, and list elements \(&lt;li&gt;\) to be green.  We could create simple-style.css with this specification:

```
/* A CSS file can have C-style comments */

/* Select by element type */ 
h1  { color: red; }
li  { color: green; }
```

We could then reference simple-style.css from within our web page \(.html\) file:

```
<head> 
  <title>Sample</title>
  <link rel="stylesheet" type="text/css" href="simple-style.css" media="screen"/>
</head>
```

Voila, we now have some gaudy and ill-considered flair!

![](img/firefox-edit-styled-step1.png)

Often we won't want to apply the same style to all elements of a given kind.  For example, we might want to apply a particular style only to paragraphs that are used for legal disclaimers.   You may recall that we included a "class" parameter with some tags in our html examples above.  We can define any "class" name we like and associate it with a style specification.  For example:

```
/* Select by class */
.disclaimer { font-size: 75%; }
.useless-extra-warning { color: red; font-weight: bold; }
```

Then we can associate one of these classes with a paragraph, and another with a limited span of text within the paragraph:

```
<p>The classic, and a modern treatment.</p>
<p class="disclaimer">Watch at your own risk. 
   The publishers of this web site accept no legal responsibility 
   for shock and fear that may be encountered by audiences of the original movie, nor 
   for puzzlement and humor that may be encountered at the second.  
   <span class="useless-extra-warning">You have been warned.</span>
```

The result:

![](img/firefox-edit-styled-step2.png)

A third way a CSS style specification can be associated with an element is by a unique identifier associated uniquely with that element.  Occasionally this is necessary, but we avoid it to the extent possible because it breaks the separation between content and style.   When we absolutely must breech that separation, we might as well do so by inserting the style specification directly in the html, like this:

```
<li>Shaun of the Dead <span style="color: black;">(2004)</span></li>
```

But avoid it when you can!

### Composing and debugging selectors

The element and class descriptors can be combined.  For example, a selector p.disclaimer would match &lt;p class="disclaimer"&gt; but not &lt;h2 class="disclaimer"&gt;.   One can also make a single specification apply to several different types of elements \(e.g., h1,h2,h3 to match the first three levels of header\), and even match elements only in certain contexts.

The many ways selectors can be specified and combined provides a lot of flexibility, but that flexibility has a price:  The developer often specifies a pattern that they expect to match certain elements in the html, but does not observe the intended effect when displaying the page.  Occasionally this is because the selector pattern does not actually match, but more often it is because some other selector pattern also matches, and the conflict between style specifications has been resolved in an unexpected way.   This is a very common problem especially when the developer is using not only their own custom style sheets but also some style sheets from external developers, which often accompany libraries like the bootstrap library for responsive design.

Fortunately all the common browsers provide debugger support for determining which style selectors have matched an element and how those styles have been combined.  For example, in Firefox we use Tools &gt;&gt; Web Developer &gt;&gt; Inspector to see the HTML together with the style information:

![](img/firefox-style-inspector.png)

Debugging selectors is still challenging, because nothing can provide an explanation of the selector that _should_ match but _doesn't_, but learning to use the built-in debugging tools in your browser will make it much more efficient than blindly changing code in your html and css files to find a combination that works.

### CSS Styling

The Cascading Style Sheets language is  large and complex, as it must be to control what amounts to a complete typesetting system.  Whole books can and have been written about it.  Nonetheless a lot can be accomplished with just a few basic features and enough understanding of the basic approach to find description of additional features in online references.  We introduce two main kinds of CSS styling:  Text styling \(typefaces, sizing, and colors\) and layout.

### CSS Text Styling

A _font_ is a typeface  with a particular set of parameters including size, weight, and variant.  For example, Times New Roman is a typeface that may be set at size 12pt, semi-bold, italic.   The distinction between a typeface and a font is often fuzzy, but it will be important to us here because CSS allows us to choose a typeface and separately choose other characteristics, and a property that is inherited from one element \(say, a typeface selected for a whole paragraph\) can interact with an explicitly set property \(like a weight or variation set in a &lt;span&gt; element within the paragraph\).

Since the actual rendering of characters in a font will take place in the browser, we must face a complication that does not often arise in typesetting for paper media.  If we were typesetting a paper document in a word processor, we would seldom choose a typeface that we did not have installed, although someone might email us a document file that called for typefaces we do not have.  In web documents, there is a much greater chance that the typefaces available to us while composing the document are not available to the user's browser.  CSS addresses this by allowing us to specify a _font family_ that includes multiple, alternative typefaces.  For example, we might want text in paragraphs to be displayed in the Helvetica typeface if it is available, but we know that Arial was basically a Helvetica clone for Windows, and if neither of those are available we could settle for any sans serif face.  We might specify

```
p { font-family: Helvetica, Arial, sans-serif; }
```

If the name of a typeface includes spaces, we can put it in quotes:

```
p { font-family: Helvetica, "Trebuchet MS", sans-serif;}
```

We can specify the size of the text in points, in screen pixels,  or relative to surrounding text.  Pixels are rarely the best choice, because they depend on screen resolution.  Points are the standard unit used in traditional typography, One point is about 0.352 millimeters, or 1/72 inch.  A typical newspaper article might be set in a 10 point font on 12 point leading \(i.e., lines spaced 12 points apart, thus leaving approximately 1 point of whitespace between lines\).

Relative sizes can be in percent \(e.g., make this text 125% the size of text in the surrounding block\), simple steps like "large" or "small" or "larger" or "smaller"\),  or in units based on the size of a character.  Character based sizes are generally in "ex"  \(the height of the character x in the surrounding text\) or "em" \(the width of the character m in the surrounding text\).  Character based sizing is especially useful for setting the size of an image or a "box" like a sidebar.  For the size of characters in a text, usually we want to set a basic overall size for the whole page, and then make text sizes within the page relative to that base size.  This makes it easy to consistently make all the text on a page a little larger or smaller.

For example, we could make the Helvetica typeface in 12 point the main font for a page, but override the color \(but no other features\) of main headers.  We could make a précis larger relative to surrounding text, indent it, and use a different typeface to set it off:

```
body { font-family: Helvetica, Arial, sans-serif;
       font-size: 12pt;
       font-weight: light;
     }

h1 { color: rgb(219, 56, 24); }

.precis {
    font-family:  Times, "Times New Roman", serif;
    font-style: italic;
    font-size: 125%; 
    margin-left:3em;   /* Width of 3 'm' in the current font. */ 
    margin-right: 3em;
```

Then we can  use  the 'precis' class to identify one paragraph to be set in the special style:

```
<body>
<h1>Zombie Mashups</h1>

<p class="precis">Like culinary fusion, zombie fusion offers
surprising combinations that work because of parallel elements
in their disparate sources.</p>

<p>All the great zombie movies contain an element of social
commentary.  A movie that combines a zombie element with another
genre or theme provides an opportunity to comment in a surprising
way, often with humor.</p>
```

The paragraph with the class parameter is then rendered in the overridden style:

![](/web-doc-structure/img/zombie-mashup-precis.png)

Since we did not specify a size for the h1 element, and since h1 elements are normally set in a size larger than the running text, this size has still been applied despite the change to its color.  The paragraph marked with the "precis" class has been made larger and indented, but the following paragraph has not.  We can do something similar by designating some citations \(marked with the &lt;cite&gt; tag in html\) as movie titles:

```
.movie-title {
    font-weight: bold; 
    color: rgb(219, 56, 24);
}
```

Then we can combine the standard html &lt;cite&gt; tag with the class "movie-title":

```
<p>Among the movies that have fused a zombie element with
a more conventional movie about young people finding their
way in the world, <cite class="movie-title">Shaun of the Dead</cite>
and <cite class="movie-title">Night of the Comet</cite>
stand out for deadpan humor.  Shaun famously
<a href="https://www.youtube.com/watch?v=9qHAOY7C1go">uses his
collection of vinyl records as weapons</a> while commenting on
which records are expendable and which should be kept.  (Dire Straits?
Throw it.)  In <cite class="movie-title">Night of the Comet</cite>,
which influenced the later <cite class="movie-title">Buffy the Vampire
Slayer</cite>, a recurring theme is the extent to which our world
has been automated and goes on without our direct involvement. 
</p>

<p>Merely mashing up genres is not enough, though.  Fusion cuisine
works because of parallels between culinary styles.  Yakisoba in place
of fettucine?  Might be interesting.  Citrus in place of vinegar?
Miso in place of parmesan?  It could work, but there is no guarantee.
A notable flop among zombie mash-ups was <cite
class="movie-title">Pride and Prejudice and Zombies</cite>, a cute
concept that dulls after after a few battles.  Perhaps it
could have been made to work with a deeper and more nuanced
integration of the <cite>Pride and Prejudice</cite> setting with
zombies as a critique of the English class system.  Simply using
zombies as <em>them</em>, a generic threatening <em>other</em>, is not
enough. </p>
```

Now the citations to movie titles get the specific treatment, but the citation to the novel _Pride and Prejudice_ does not:

![](/web-doc-structure/img/zombie-mashups-cites.png)

### Page Layout in CSS

We use style sheets not only to format the text on a page, but also to lay out the page itself.  In an organization large enough to have specialists in graphical design and document design, the page layout as well as many formatting details are likely to be designed by specialists.   When specialists are not available, we often start from a standard style \(e.g., one of the many open source designs available from the [Open Source Web Design](http://oswd.org) community\).  In either case, though, software developers must understand the mechanics of page layout to use the provided styles competently and to troubleshoot them when the effects are surprising.

Approaches to page layout have evolved with the web.  In the bad old days, before CSS provided sufficient control of layout, many web sites used designs based on html tables.   Table-based layout was supplanted by CSS controls soon after they were introduced, but designers struggled in particular to create multi-column page layouts that adjusted in robust ways to changes in window size, and especially to be "responsive" to the formatting needs of  mobile devices with small screens.  Two new options were introduced with CSS-3:  Grid-based design \(a widely used approach for paper document design\) and row- and column-based "flexible box" design.  We illustrate with the flexible box model.

Let us suppose we want a navigational sidebar to the left of the main document content, with a page header and footer that extends across the page.

![](/web-doc-structure/img/page-layout-normal.png)

This layout may not work well if squeezed too far. If the page is too narrow, we want the navigation links to be stacked atop the page body.

![](/web-doc-structure/img/page-layout-narrow.png)

A window that is too wide may also present a problem.  To read text, muscles in the human eye must steer the field of vision from the end of one line to the beginning of the next.  These muscles have limited accuracy.  While several factors, including the white space between lines, affect the ability to make this move accurately, as a rule of thumb lines of text should not be more than about 50 to 60 characters in length \(about 30-50 em\).  When the page is wide enough for the main text column to exceed this width, we prefer to pad it on the right rather than extend the length of text lines.

![](/web-doc-structure/img/page-layout-wide.png)

We can easily specify a flexible box layout that dynamically achieves all three layouts as the size of the browser window is adjusted.  Of course we'll need to indicate which parts of the html belong to the page heading, sidebar, and foot.

```
<body>
<div class="pagehead">
    <h1 class="page-title">Zombie Mashups</h1>
</div>

<div class="content">
<div class="sidebar">
  <h1>You may also enjoy</h1>
  <!-- sidebar content here -->
</div> <!-- sidebar -->

<div class="main-content">
  <!-- main content here -->
</div> <!-- main-content -->
</div> <!-- content -->

<div class="pagefoot">
  <p>Publisher accepts no responsibility for anything at all</p>
</div>

</body>
</html>
```

In the CSS file we will specify that the 'content' division is a flexible box

```
div.content {
    display: flex;     /* Rows and columns */
    flex-wrap: wrap;   /* Stack if too narrow */
}
```

The 'flex-wrap' declaration is necessary to let the second column "wrap" to be stacked below the sidebar if necessary.  When is it necessary?  We'll need to specify minimum widths for them ... and at the same time we can specify preferred width \(in %\), and maximum width \(in em, the unit we'll also use for the minimum\).

```
div.content .sidebar {  /* Read as "navbar within content" */
    padding: 1ex; 
    background-color: rgb(240,240,240);
    /* Width of section */
    width: 15%;       /* Ideal, within limits ... */ 
    max-width: 20em; 
    min-width: 10em;  /* Wrap if it would be narrower */ 
    flex-grow: 1;     /* Half as stretchy as main column */
    flex-shrink: 2;   /* Twice as compressible as main column */ 
}

div.content .main-content {
    padding-left: 1em;
    padding-right: 1em; 
    /* Ideal size, limits, and stretchiness */
    width: 75%;
    max-width: 40em;
    min-width: 20em;
    flex-grow: 2;
    flex-shrink: 1;
}
```

We use the em unit \(width of character 'm'\) for minimum and maximum sizes, rather than pixels or percent, because readability is closely related to the size of characters.  If we change the size of the font, minimum and maximum widths specified in em will be adjusted to match.

We have also specified flex-grow and flex-shrink to control how the column sizes change when they are between their minimum and maximum values.  In the example, we have chosen to make the main content column stretch twice as fast as the sidebar when the window is expanded, and to make the sidebar shrink twice as fast as the main content column when the window is narrowed below the ideal range ... but only until both have reached their minimum widths, at which point the row "wraps" and the sidebar becomes instead a vertical section:

![](/web-doc-structure/img/zombie-sidebar-packed-thumb.png)

When both can fit at their ideal ratio, the page is laid out as shown above.  If we widen the browser window beyond the maximum width of both columns, padding is added on the right as shown below:

![](/web-doc-structure/img/zombie-sidebar-padded-thumb.png)

And if the browser window becomes too narrow for the two columns at their respective minimum widths, the row wraps:

![](/web-doc-structure/img/zombie-sidebar-stacked-thumb.png)

The full source code for this page is available: [HTML](/web-doc-structure/Samples/zombie-sidebars.html "HTML source code for zombie mashups page") and [CSS](/web-doc-structure/Samples/zombie-sidebars.css "Style sheet for Zombie Mashups"). There are a couple of other features worth noting. First, we can write CSS selectors that combine elements like h1 and classes like page-title, and we can even select elements by the way they are nested.  For example, "div.content .sidebar" selects elements with class "sidebar" that are within division with class "sidebar":

```
div.content .sidebar {  /* Read as "navbar within content" */
    padding: 1ex; 
    background-color: rgb(240,240,240);
    /* Width of section */
    width: 15%;       /* Ideal, within limits ... */ 
    max-width: 20em; 
    min-width: 10em;  /* Wrap if it would be narrower */ 
    flex-grow: 1;     /* Half as stretchy as main column */
    flex-shrink: 2;   /* Twice as compressible as main column */ 
}
```

We can also give one selector to match more than one combination class, element, or combination.  A comma in a selector can be read as "or":

```
h1.page-title, div.pagefoot p  {
    text-align:  center;
    padding: 1ex;
    margin-bottom: 0.5ex; 
}
```

The style above applies to an h1 element with class page-title, and applies also to a p \(paragraph\) element that appears within a division with class pagefoot.

---

### Where to learn more

There are books and books and whole degree programs about designing documents that are attractive and effective in communicating information, and more books on achieving those designs with HTML and CSS.  The former field is called "information design".  Much of the published work is focused on visualization of quantitative data, but it is at least equally useful for a developer of end-user software \(on the web and in other media\) to become familiar with some basics of typography and readability.   One reasonable starting point is the [material design guidelines](https://material.io/design/color/text-legibility.html) distributed by Google.  \([Help us out:](mailto: michal@cs.uoregon.edu&subject=322-book typography intros) The authors of this text would appreciate recommendations of other good, relatively concise starting points with emphasis on fundamental principles of typographic design.\) 

For reference and clear explanation of CSS, we recommend the [Mozilla Developer Network documentation](https://developer.mozilla.org/en-US/) available online, including the [clearest documentation of the flex-box layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox) that we are aware of. 





