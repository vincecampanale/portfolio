---
layout: post
title:  "Using the Vignere Cipher to Encrypt a Message (Part 2)"
date:   2017-02-01
---
This post is Part 2 of a three part series:
[Using the Vignere Cipher to Encrypt a Message (Part 1)](http://www.vincecampanale.com/blog/2017/01/20/vignere-cipher-part1/)
Using the Vignere Cipher to Encrypt a Message (Part 2) <-- You are here
[Using the Vignere Cipher to Encrypt a Message (Part 3)](http://www.vincecampanale.com/blog/2017/02/06/vignere-cipher-part3/)

In [Part 1](http://www.vincecampanale.com/blog/2017/01/20/vignere-cipher-part1/), I gave a brief introduction to the Vignere cipher and how it works. I mentioned that there are two ways to go about emulating the Vignere cipher with a program. One of them involves essentially replicating the Caesar cipher, which I covered in [this post](http://www.vincecampanale.com/blog/2017/01/20/caesar-cipher/), except with a dynamic shift number based on the keyword. The other method involves a Vignere table. In this post, I'm going to cover the first approach: the pseudo-Caesar cipher method.

This is a really fun problem and it takes a little while to get into, but once you get a grasp on what's happening, it's extremely rewarding.

Onwards!

#### Psuedocode

Outlining our approach in psuedocode would look something like this:
{% highlight javascript %}
//Create a function that takes two variables: a message and a keyword (both strings)
//Make a string by repeating the keyword over and over again
//Get the portion of the repeated keyword string that is as long as the message argument
//Encode the message with the keyword
  //split the message into an array
  //replace each element of the message array with the right letter
  //join that array back together
//return the new string of encoded characters
{% endhighlight %}

#### Diving In

Let's make our basic function first, then deal with the details of how to encode it after.

{% highlight javascript %}
function vignere (message, keyword = "lemon") { //default keyword is lemon
  for ( var i = 0; i < message.length; i++ ) {
    keyword += keyword; //repeat the keyword the number of times the string is long
  }
  const keywordStr = keyword.substr(0, message.length);
  const ciphertext = encode(message, keywordStr); //figure out this function in next step
  return ciphertext;
}
{% endhighlight %}

So, in this block, we have laid the groundwork for our vunderful program. Now, we need to give the `encode()` function some function-ality!

#### To encode or not to encode?

We'll start by asking ourselves what we want this function to accomplish. We want it to take in two parameters: a message, and a string with the same length as message consisting of a keyword repeated over and over again. Then, we want to do some magic to these inputs, and essentially layer them together to create an encoded word.

In order to do this "layering," we will use the Caesar cipher approach. We'll get the character code of each letter in the message, the character code of the corresponding letter in the keyword string (at the same index), combine these character codes, and presto change-o this cumulative character code into a new letter: our cipher letter. Do this for each letter in the message, and we have our cipher text.

Now, in code:

{% highlight javascript %}
function encode (message, keywordStr) {
  const baseArray = message.split(''); //call it the base array because we will be adding the keyword character code "on top" of each character
  baseArray.forEach(function addKeywordToBase(letter, index){ //decided to give this callback a name in case something goes wrong, it can be easily debugged
    let baseCharCode = letter.charCodeAt(0);
    let keywordedCharCode = keywordStr[index].charCodeAt(0);
    let combinedCharCode = baseCharCode + keywordedCharCode - 97; //subtract 97 to prevent double counting of the first 97 char codes (since 'a' is charcode 97)
    baseArray[index] = String.fromCharCode(
      combinedCharCode > 122 ? combinedCharCode - 26 : //wrap back around alphabet
      combinedCharCode < 97  ? combinedCharCode + 26 : //same as above line, on other side of alphabet
      combinedCharCode //if in range, just return the value
    );
  });
  return baseArray.join('');
}
{% endhighlight %}

See where I used the `let` keyword in the callback to the `.forEach` method? To be honest, I'm not quite sure when it's appropriate to use `let` vs. when not to. Perhaps that's a good topic for a future post. I know it has something to do with block scoping and the variable getting "trashed" after it's used, so it doesn't waste memory. Based on these two pieces of information, I think it's safe to say that it's best to use `let` when you have a for loop or a higher order function and want to generate a temporary, local variable to operate on, such as in this case.

Anyways, back from tangent town -- this function looks a lot like the Caesar cipher because it is, in fact, a Caesar cipher. Nothing really new here, so in the interest of keeping this blog DRY, I'd recommend checking out [this post]() to learn more about how this code works.

In [Part 3](http://www.vincecampanale.com/blog/2017/02/06/vignere-cipher-part3/), things get a little more interesting. I'll go over how to solve this problem using a Vignere table instead.

Til next time, Vinny out.
