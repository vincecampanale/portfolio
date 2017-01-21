---
layout: post
title:  "Solved: The Caesar Cipher"
date:   2017-01-20
url: http://www.vincecampanale.com/blog/2017/01/05/caesar-cipher/
---

[According to learncryptography.com](https://learncryptography.com/classical-encryption/caesar-cipher), the Caesar Cipher (aka a shift cipher), is a very simple form of encryption "where each letter in the original message is replaced with a letter corresponding to a certain number of letters up or down in the alphabet."

In this solution, I step through my approach to constructing a Javascript function that emulates the Caesar Cipher. The function takes two arguments: the string to encrypt, and the shift constant. The shift constant determines how many letters we go up (if positive) or down (if negative) in the alphabet to replace each letter. For example, `caesarCipher("hello", 5)` should return `"mjqqt"`.


#### Outlining & Pseudocode
To start, let's construct the shell of the function and get a sense for how it should work in the end.
~~~ javascript
function caesarCipher(str, shiftNum) {
  //split the input string into an array to operate on each element
  const strAsArray = str.split("");
  //create a dummy array to push the shifted elements into
  const shifted = [];

  //instantiate the function that will shift each letter   
  function shift(letter) {
    //TODO: Shift the letter
    shifted.push(letter); //push the shifted letter to the shifted array
  }

  //use higher order function .forEach to shift each letter in strAsArray
  strAsArray.forEach(shift);

  return shifted.join(""); //return shifted array as a string
}
~~~

#### Shifting a Letter
We're going to use the ASCII code to shift the letters. We'll use the [`.charCodeAt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt) method in order to get the ASCII code of each letter. Since all the letters of the alphabet are in order of ascending ASCII codes, all we have to do is add `shiftNum` to the character code of the current letter to get the new letter's code. Then, we can use [`.fromCharCode()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode) to convert back to the letter version.

In code,
~~~ javascript
function caesarCipher(str, shiftNum) {
  //split the input string into an array to operate on each element
  const strAsArray = str.split("");
  //create a dummy array to push the shifted elements into
  const shifted = [];

  //instantiate the function that will shift each letter
  function shift(letter) {
    letter = String.fromCharCode(letter.charCodeAt(0)+shiftNum);
    shifted.push(letter); //push the shifted letter to the shifted array
  }

  //use higher order function .forEach to shift each letter in strAsArray
  strAsArray.forEach(shift);

  return shifted.join(""); //return shifted array as a string
}
~~~

Note that the callback function `shift` operates on one letter at a time, so we want the character code of the only letter in a one-letter string, which is at index 0, hence `letter.charCodeAt(0)`.

#### But Wait, There's More.
What if we want to shift `"zebra"` by 3 characters? The way our function is set up right now, we'd end up with `"}jgwf"` because the character code for `122 + 3` corresponds to the symbol `"}"`. Likewise, if applying the shift constant results in a character code below 97, we'll end up with a capital letter rather than the correctly shifted one.

Let's do some math. If the resulting shifted code is greater than 122, we want to take the difference between our sum and 122, and add that to 97. Likewise, if the resulting shifted code is less than 97, we want to take the difference between our sum and 97, and subtract that number from 122. Translation:

~~~ javascript
const sum = letter.charCodeAt(0) + shiftNum;
if (sum > 122) {
  letter = String.fromCharCode(97 + (sum - 122) - 1); //the -1 is to adjust for inclusion
} else if (sum < 97) {
  letter = String.fromCharCode(122 - (97 - sum) + 1); //the +1 mirrors the -1 above
} else {
  letter = String.fromCharCode(sum);
}
~~~

Let's drop that into our function.

~~~ javascript
function caesarCipher(str, shiftNum) {
  //split the input string into an array to operate on each element
  const strAsArray = str.split("");
  //create a dummy array to push the shifted elements into
  const shifted = [];

  //instantiate the function that will shift each letter
  function shift(letter) {
    const sum = letter.charCodeAt(0) + shiftNum;
    if (sum > 122) {
      letter = String.fromCharCode(97 + (sum - 122) - 1); //the -1 is to adjust for inclusion
    } else if (sum < 97) {
      letter = String.fromCharCode(122 - (97 - sum) + 1); //the +1 mirrors the -1 above
    } else {
      letter = String.fromCharCode(sum);
    }
    shifted.push(letter); //push the shifted letter to the shifted array
  }

  //use higher order function .forEach to shift each letter in strAsArray
  strAsArray.forEach(shift);

  return shifted.join(""); //return shifted array as a string
}
~~~

#### Refactor & Get Creative
I think it makes more sense syntactically to append this method to the String prototype, so it can be used like "`"hello".encryptWithCaesars(3)` where `"hello"` is the string to be encrypted and the argument passed to the method is the shift constant.

I made a couple of other changes to save memory, and flex my ternary operator, arrow function, ES6, and syntax muscles. Check it out:

~~~ javascript
//This adds a new method to the String prototype which implements the function "encryptWithCaesars" on that string.
//The method takes one parameter, "shiftNum", for which the default value is 2 (if no shiftNum is provided).
//Saves memory by replacing the letters in place, rather than creating a new array and pushing the letters to that array.
String.prototype.encryptWithCaesars = function encryptWithCaesars(shiftNum = 2) {
  const strAsArray = this.split("");
  strAsArray.forEach((letter) => {strAsArray[strAsArray.indexOf(letter)] =
    String.fromCharCode(
      letter.charCodeAt(0) + shiftNum > 122 ? (letter.charCodeAt(0)+shiftNum) - 26 :
      letter.charCodeAt(0) + shiftNum < 97 ? (letter.charCodeAt(0)+shiftNum) + 26 :
      letter.charCodeAt(0) + shiftNum
    )
  });
  return strAsArray.join("");
}
//Note: this solution only works with lowercase letters.
~~~

Let me know what you think! Contact info in the footer - tweet at me with suggestions/comments.
Hope I was able to pass on a little knowledge and help you out with this problem!

Adios.
