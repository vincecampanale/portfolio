---
layout: post
title:  "Solved: Symmetric Difference of Multiple Arrays (Part 4)"
date:   2017-01-12
url:  /blog/2017/01/12/symmetric-difference-of-multiple-arrays-part4/
---
This post is Part 4 of a four part series:  
[Symmetric Difference of Multiple Arrays (Part 1)](http://www.vincecampanale.com/blog/2017/01/03/symmetric-difference-of-multiple-arrays-part1/)  
[Symmetric Difference of Multiple Arrays (Part 2)](http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part2/)  
[Symmetric Difference of Multiple Arrays (Part 3)](http://www.vincecampanale.com/blog/2017/01/10/symmetric-difference-of-multiple-arrays-part3/)  
Symmetric Difference of Multiple Arrays (Part 4) <-- You are here

The final post! The one where we get to put it all together and see the function in all it's glory.

Let's recap what we have:

#### Helper Function 1: Symmetric Difference of Two Arrays
This function takes in two arrays and returns their symmetric difference as an array.
See more detail on how this was constructed in [Part 2]().

~~~ javascript
function symDiffTwoArrays(arr1, arr2) {
  //filter a by checking for elements in b
  function filterArray(a, b){
    return a.filter(function isInOtherArray(element){
      return b.indexOf(element) === -1;
    });
  }

  //remove elements from arr2 that are in arr1
  var filteredArr1 = filterArray(arr1, arr2);

  //remove elements from arr2 that are in arr1
  var filteredArr2 = filterArray(arr2, arr1);

  //concatenate filtered arrays
  var symDiff = filteredArr1.concat(filteredArr2);

  //return concatenated array
  return symDiff;
}
~~~

#### Helper Function 2: Remove Duplicates from an Array
This function takes in an array and returns that same array with no duplicates.
See more detail on how this was constructed in [Part 3]().

~~~ javascript
function removeDuplicates(arr) {
  return arr.filter(function(item, pos) {
    return arr.indexOf(item) == pos;
  });
}
~~~

Okay, now it's time to bring `.reduce()` in and finish this function once and for all.

#### Applying [`.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

The MDN docs say "The reduce() method applies a function against an accumulator and each value of the array (from left-to-right) to reduce it to a single value." Don't worry, I don't understand that either.

I'll try my own explanation, and if this doesn't make sense, try googling "javascript .reduce" and you will find lots of useful tutorials and resources.

Here it goes: `.reduce()` implements a callback function which takes two *primary* arguments (there are other options but there are two really important ones). These two arguments are two elements from the array on which you are calling the `.reduce()` method. The first argument is the first element in the array, and the second argument is the second element in the array. The callback function acts on both of these arguments, and returns a single *thing*, (the return value of your callback function) this can be an object, number, array, or whatever. This *thing* is referred to as the "accumulator." The accumulator is then used as the first argument for the next round of the callback function. The second argument for the callback function comes from the next element in the array (which was not part of the first callback round). This process repeats until the entire array has been processed and the finaly value of the accumulator is returned.

So, in our example, the callback function is going to be our symmetric difference of two arrays function, so aptly named `symDiffTwoArrays(arr1, arr2)`. Calling `.reduce()` on the `argArray` array will apply the symmetric difference algorithm to the first two elements, then the result of that gets stored in the "accumulator". Then, symmetric difference algorithm gets applied to the accumulator and the next element, and so on. Here's a pretty picture hand-drawn by moi:

<a href="http://tinypic.com?ref=2m4vkli" target="_blank"><img src="http://i66.tinypic.com/2m4vkli.jpg" border="0" alt="Image and video hosting by TinyPic"></a>

Hopefully this shed some light on the seemingly-magical `.reduce()` for you.

#### Put It All Together
Now that we have covered each part in detail, the whole should make sense.

~~~ javascript
function sym(args) {

  //get symmetric difference of two arrays
  function symDiffTwoArrays(arr1, arr2) {
    //filter a by checking for elements in b
    function filterArray(a, b){
      return a.filter(function isInOtherArray(element){
        return b.indexOf(element) === -1;
    });
  }

  var filteredArr1 = filterArray(arr1, arr2); //filter arr1
  var filteredArr2 = filterArray(arr2, arr1); //filter arr2
  var symDiff = filteredArr1.concat(filteredArr2); //concatenate filtered arrays
  return symDiff; //return concatenated array
  }

  //remove duplicates from an array
  function removeDuplicates(arr) {
    return arr.filter(function(item, pos) {
      return arr.indexOf(item) == pos;
    });
  }


  //PUT IT TOGETHER
  //use array.from trick to get arguments into an array
  var argArray = Array.from(arguments);
  //use reduce method to get the symmetric difference of all arrays
  var symDifference = argArray.reduce(symDiffTwoArrays).sort();
  //remove duplicates from the resulting array
  var symDiffNoDups = removeDuplicates(symDifference);


  return symDiffNoDups;
}
~~~

Thanks for reading!
Peace.
