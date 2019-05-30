---
title: Cheat Sheet
date: "2019-05-30T01:57:30+00:00"
---

*Emphasize*

**Strong**

```javascript{numberLines: true}
// In your gatsby-config.js
plugins: [
  {
    resolve: `gatsby-transformer-remark`,
    options: {
      plugins: [
        `gatsby-remark-prismjs`,
      ]
    }
  }
]
```

```{numberLines: 549}
...
a long imaginary code block
...
```

```js
{
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/src/pages`,
        name: "pages",
      },
    }

~~~~
This is a 
piece of code 
in a block
~~~~
`This is code`
```
This too
```

Syntax Highlighting
```css
#button {
    border: none;
}
```

Definitions
:  Text-to-HTML conversion tool

# Header 1
## Header 2
### Header 3 
#### Header 4
##### Header 5
###### Header 6

Hover Text

*[HTML]: HyperText Markup Language

A [link](http://example.com "Title").

Some text with [a link][1] and
another [link][2].
[1]: http://example.com/ "Title"
[2]: http://example.org/ "Title"

Inline Image: ![Alt](https://i2.wp.com/s.wordpress.org/about/images/logos/wordpress-logo-32.png?zoom=2 "Title")

Linked Image: [![alt text](https://i2.wp.com/s.wordpress.org/about/images/logos/wordpress-logo-32.png?zoom=2)]
(http://wordpress.com/ "Title")

Footnotes

I have more [^1] to say up here.

[^1]: To say down here.

Bullets
* Item
* Item
- Item
- Item

Numbered List
1. Item
2. Item

Blockquotes
> Quoted text.
> > Quoted quote.

 	

  Begin each line with 
  two spaces or more to 
  make text look
  e x a c t l y 
  like  you  type i
  t.




This is my first post on my new fake blog! How exciting!

I'm sure I'll write a lot more interesting things in the future.

Oh, and here's a great quote from this Wikipedia on
[salted duck eggs](http://en.wikipedia.org/wiki/Salted_duck_egg).

> A salted duck egg is a Chinese preserved food product made by soaking duck
> eggs in brine, or packing each egg in damp, salted charcoal. In Asian
> supermarkets, these eggs are sometimes sold covered in a thick layer of salted
> charcoal paste. The eggs may also be sold with the salted paste removed,
> wrapped in plastic, and vacuum packed. From the salt curing process, the
> salted duck eggs have a briny aroma, a gelatin-like egg white and a
> firm-textured, round yolk that is bright orange-red in color.

![Chinese Salty Egg](./salty_egg.jpg)
