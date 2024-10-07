# Learning SASS
## Chapter 1: The Basics:

It is an extension of the CSS, the full form being Syntactically Awesome Style Sheets. 

### Why should I learn SASS?

SASS has support for variables (Standard CSS does too) which may not seem much on the surface but SASS also has support for inheritance, nested rules, imports and mixins which as a whole package makes it **DRY** and more powerful than standard CSS.

<aside>
💡

It has two extensions, the SCSS syntax (.scss) which is the most common one used and considered a superset of CSS and .sass which is more python like, following indentation rules instead of the bracket system.

</aside>

Of course the Browser does **NOT** support .scss files, which is why it must be converted to standard CSS, which is done with the help of the **preprocessor**.

## Following Examples:

1. Install the “Live SASS compiler” in VS Code.
2. Click on the “Watch SASS” button on the bottom bar to activate it.

## Chapter 2: SASS Variables

As mentioned before CSS does have support for variables such as the following example:

```css
:root {
	--primary-color: blue;
	--secondary-color: red;
}

body {
	background-color: var(--primary-color);
	color: var(--secondary-color);
}
```

In SASS variables are declared with the preceding “$” sign, such as the following,

```scss
$header-color: blue;
$text-color:red;
```

Variables in SASS just like in Javascript can store a range of primitives such as strings, bools, numbers, colors, lists, nulls. Thus these variables can used anywhere in the CSS file and doesn’t need to changed at every place, only at the variable declaration.

They can be easily applied anywhere for example,

```scss
header {
	background-color: $header-color;
	color: $text-color;
}
```

## Chapter 3: Nesting in SASS

Usually when targeting nested components in regular CSS, it essentially looking like this, 

```css
nav {
	margin-top: 15px;
}
nav ul {
	list-style-type:none;
	display:inline-block;
	margin-left: 5px;
}
nav ul li {
	padding: 0 8px;
	display:inline-block;
}
nav ul li a {
	padding: 1px 3px;
	border-radius: 8px;
	color:white;
	transition: 300ms;
}
nav ul li a:hover {
	background-color: lightblue;
}
```

While this is feasible for a simple project, it can become tedious to do so if there are deeply nested elements in the DOM.

SASS allows a much more readable nesting syntax such as the following:

```scss
nav {
	margin-top:15px;
	ul {
		list-style-type:none;
		margin-left: 5px;
		li {
			display:inline-block;
			padding: 0 8px;
			a {
				padding: 1px 3px;
				border-radius:8px;
				color:white;
				transition:300ms;
			}
			a:hover {
				background-color: lightblue;
			}
		}
	}
}
```

This syntax makes it much more readable and becomes clearer as to what styles are being applied to a nested DOM element.

## Chapter 3: Partials, “@use” and “@forward” And The “@as” keyword

Partials are a way to conduct a separation of concerns, avoiding conflicts, each .scss file can only have concerns regarding a particular element or component which can then be reused and imported to a main.scss file.

### A Project Example:

```
styles
    |
    base
        |
        _index.scss
        _reset.scss
        _typography.scss
    utils
        |
        _utils.scss
    styles.scss
```

<aside>
💡

When making partials, files must start with an “_” such in the ‘base’ folder.

</aside>

In the ***styles.scss*** file, we can then import the rules from the “**_reset.scss**” file by adding a “**@forward**” at the top of our main (styles.scss) file. The “**_typography.scss**” will contain rules or variables related to the typography of the entire app and thus must also be imported using the “@forward” keyword. 

The “***_index.scss***” file will import all the other files in the “/base/” folder this way we in the main styles.scss file we will only need to import the “/base/index” file thus making our main .scss file look much cleaner.

The “/utils/” folder is where we keep our variables, mixins, loops etc. Thus we can create files such as “_variables.scss” where we can store our reusable values such as colors, sizes etc. 

```scss
$header-color:blue;
$main-color:lightblue;
```

Now since these are variables that have to be re-used we do the following in our main styles.scss file,

```scss
@use "/utils/index";
```

Provided that we created an “_index.scss” file in our “/utils/” folder where we forwarded the “_variables” file. Now to access this saved variables we have to do the following in our main file.

```scss
...
header {
	background-color: index.$header-color;
}
```

The “index” word in this case is the the last name in the path of the “**@use**” keyword path. Thus we we can use all of the variables that were declared in the “/utils/_variables.scss” file. Now to solve the issue of each separate folder being forwarded from their respective “_index.scss” files we can use the “as” keyword similar to how it was used in Javascript/Typescript. 

```scss
@use "/utils/index" as variables;
```

Thus we can then use the “variable” word to get access to our variables such as:

```scss
header {
	background-color: variables.$header-color;
}
```

## Chapter 4: What Are Mixins?

A *mixin* is a group of reusable styles such as the following:

```scss
@mixin paragraph-style {
	font-size: 1rem;
	text-align:justify;
}
```

This style can then be reused in any other style declaration such as if there is a card component that has a paragraph then we resuse this style by doing the following:

```scss
.card {
	@include paragraph-style;
}
```

We use the “**@include**” to add the mixin to our “.card” component.

Mixins can take in variables to use, this ultimately lets us create abstract classes that can have vastly different design looks based on whatever variables get passed on to the mixin. Thus if we want to have different kinds of callouts in our pages, we design a generic callout-style such as: 

```scss
@mixin callout-style ($fontSize, $marginLeft, $bgColor) {
	font-size: $fontSize;
	margin-left:$marginLeft;
	background-color:$bgColor;
}
```

This mixin can then be reused in for creating multiple different kinds of callouts such as:

```scss
.callout-1 {
	@include callout-style (1.6rem, 20px, lightgrey);
}
.callout-2 {
	@include callout-style (1.2rem, 10px, lightblue);
}
.callout-3 {
	@include callout-style (1.5rem, 15px, lightgreen);
}
```

## Chapter 5: “@extend” Keyword

The “**@extend**” allows us to inherit the properties of a previously defined selector, for example we can define a generic `.btn` class that has the following:

```scss
.btn {
	padding: 10px;
  border-radius: 8px;
  font-family: monospace;
  font-size: 0.95rem;
}
```

We can then re-use this predefined class on another selector , for example in a CTA section there are two buttons present side-by-side, ‘Reccomendations’ and ‘Learn More’, we apply this generic `.btn` class to our CTA buttons as follows:

```scss
.btn-recc {
	@extend .btn;
	....
}

.btn-learn {
	@extend .btn;
	....
}
```

This allows us to apply re-apply the generic styles we defined keeping our code DRY and allows us to apply different styles to the two different buttons as we desire.

## Chapter 6:  “@If” , “**@else if**” **and “@else”**

SASS has support for conditional logic for when we are reusing a predefined class and then we can pass in variables to change the values of a given property. Such as for example, 

```scss
@mixin setFontSize ($type) {
	@if $type == small {
		font-size: 1.2rem;
	}
	@else if $type == medium {
		font-size:1rem;
	}
	@else {
		font-size: 0.95rem;
	}
}
```

This mixin can then be applied to a reusable mixin for example, 

```scss
@mixin nav-style(
  $marginLeft,
  $linkColor,
  $visitedColor,
  $bgColor,
  $padding,
  $navType,
  $display
) {
  nav {
    margin-left: $marginLeft;
    ul {
      list-style-type: none;
      li {
        display: $display;
        margin-inline: 10px;
        margin-top: 5px;
        a {
          color: $linkColor;
          text-underline-offset: 5px;
          padding: $padding;
          @include setFontSize($navType);
          margin: 16px 1px;
          transition: 300ms;
          border-radius: 8px;
          background-color: $bgColor;
        }
        a:hover {
          background-color: white;
          color: black;
        }
        a:visited {
          color: $visitedColor;
        }
      }
    }
  }
}
```

in the link element we are using the `setFontSize` mixin that we defined earlier and thus we use the `nav-style` anywhere else we can specify what should the font be by setting the variable name.

## Chapter 7: For Loops And While Loops

For and While loops work exactly the same as they do in any other programming language, the syntax for them being:

### For Loops:

```scss
@for $i from 1 through 12 {
	.col-#{$i} {
		width: (100% / 12) * $i; 
	}
} 
```

The only thing to know is the “to” keyword doesn’t include the ending number while the “through” keyword does, if it was “to” above then we would need to add 1 to the ending number.

### While Loops:

```scss
$i = 1;
@while $i < 13 {
	.col-#{$i} {
		width: (100% / 12) * $i;
	}
	$i: $i + 1;
}
```

## Chapter 9: Looping Over A List with “@each”

We can use the “@each” keyword to create a number of classes based on items in a list, for example:

```scss
$colors : (h1:blue,h2:lightblue,h3:lightgreen)

@each $key,$value in $colors {
	.#{key}-color {
		color: $value;
	}
}
```

This will create the following the classes after compiling:

```css
.h1 {
	color:blue;
}
.h2 {
	color: lightblue;
}
.h3 {
	color: lightgreen;
}
```