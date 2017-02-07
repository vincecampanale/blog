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
4) Put it all together in the final function  
```

### Step 1: Build the `generateAlphabet` function

In this function, the parameter will be a starting index. We are going to iterate over twenty-six char codes, starting at the provided start index. Presumably, the first char code will be 97, and they will go up from there. In order to account for char codes over 122, we add some if/else logic into the `String.fromCharCode` method. Ternary operators allow us to keep this code succinct.

```js
function generateAlphabet(start) {
  let alphabet = [];
  //from start index to 26 chars later
  for (let i = start; i < start + 26; i++) {
    //convert the char code into a letter and push it to the alphabet array
    alphabet.push(String.fromCharCode(
      i > 122 ? i - 26 : //if char code > 122, return code - 26, else
      i < 97  ? i + 26 : //if char code < 97, return code + 26, else
      i                  //just return the code
    ));
  }
  return alphabet; //return the alphabet array
}
```

### Step 2: Build the `generateVignereTable` function

Dedicating a function to generating alphabets with different starting character codes allows us to keep the Vignere table function surprisingly simple.

All we need to do is instantiate an empty object, `table`. Load the keys of that object up with the standard alphabet, starting with the letter 'a' (char code 97). Then for each key in the table, we generate an alphabet that starts at the key's index. So the second key ('b') has an alphabet starting with b and wrapping back around to end with a. The third key ('c') has an alphabet starting with c and wrapping back around to end with b. And so on.

In code:
```js
//generate an object, where each key is a letter and each value is another alphabet
function generateVignereTable() {
  let table = {}; //instantiate a temporary object to hold the table
  table.keys = generateAlphabet(97); //set the keys of the object equal to the standard alphabet (starting at 97)
  table.keys.forEach((key, index) => { table[key] = generateAlphabet(97 + index) });  //set the value of each key as the alphabet
  return table; //return the table
}
```
