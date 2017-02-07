---
layout: post
title:  "Using the Vignere Cipher to Encrypt a Message (Part 3)"
date:   2017-02-06
---
This post is Part 3 of a three part series:
[Using the Vignere Cipher to Encrypt a Message (Part 1)]()
[Using the Vignere Cipher to Encrypt a Message (Part 2)]()
Using the Vignere Cipher to Encrypt a Message (Part 3) <-- You are here

In [Part 1](), I gave a brief overview of the Vignere cipher and discussed the two approaches to solving it (the two approaches that I could come up with - there are definitely others). In [Part 2](), I covered the first approach, which is essentially a Caesar cipher with a dynamic shift number. In this part, I'm going to step through the more interesting solution - the way it's really intended to be done - using the magical Vignere table.

A Vignere table looks like this:  

<img
  src = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Vigen%C3%A8re_square_shading.svg/864px-Vigen%C3%A8re_square_shading.svg.png"
  style = "width: 600px;
           height: 600px;"
/>

###### By Brandon T. Fields (<a href="//commons.wikimedia.org/w/index.php?title=User:Cdated&amp;action=edit&amp;redlink=1" class="new" title="User:Cdated (page does not exist)">cdated</a>) - Based upon <a href="//commons.wikimedia.org/wiki/File:Vigenere-square.png" title="File:Vigenere-square.png">Vigenere-square.png</a> by <a href="https://en.wikipedia.org/wiki/User:Matt_Crypto" class="extiw" title="en:User:Matt Crypto">en:User:Matt Crypto</a>. This version created by <a href="//commons.wikimedia.org/wiki/User:Bdesham" title="User:Bdesham">bdesham</a> in Inkscape, and modified by <a href="//commons.wikimedia.org/w/index.php?title=User:Cdated&amp;action=edit&amp;redlink=1" class="new" title="User:Cdated (page does not exist)">cdated</a> to include visual guides.<a href="//commons.wikimedia.org/wiki/File:Inkscape_Logo.svg" title="File:Inkscape Logo.svg"></a>This <a href="https://en.wikipedia.org/wiki/Vector_images" class="extiw" title="w:Vector images">vector image</a> was created with <a href="//commons.wikimedia.org/wiki/Help:Inkscape" title="Help:Inkscape">Inkscape</a>., Public Domain, <a href="https://commons.wikimedia.org/w/index.php?curid=15037524">Link</a>

Don't worry about deciphering this now, you'll gain a deeper understanding as I go over the code to build this thing.

The process breaks down into four primary functions: the `generateAlphabet` function, the `generateVignereTable` function, the `encodeWithTable` function, and of course the `vignereCipherWithTable` function.

In high-level pseudocode, we want to:
```
1) Generate an alphabet starting with a given letter (a in the first column, b in the second, etc) - note: the alphabet must wrap around to the beginning when it reaches z
2) Generate a Vignere table
  - The keys consist of the standard alphabet (a-z)
  - Each key's value is an alphabet starting with that key and wrapping back   around to a (each value is 26 letters)
3) Encode the message by looking up each letter of the original message in the   keys of the Vignere table, then traversing the table to get the value from the   character code of the keyword letter  
4) Finally, put it all together in the final function  
```

### Step 1: Build the `generateAlphabet` function
