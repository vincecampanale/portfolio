---
layout: post
title:  "Solved: Symmetric Difference of Multiple Arrays (Part 2)"
date:   2017-01-05
url: http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part2/
---
This post is Part 2 of a four part series:  
[Symmetric Difference of Multiple Arrays (Part 1)](http://www.vincecampanale.com/2017/01/03/symmetric-difference-of-multiple-arrays-part1/)  
Symmetric Difference of Multiple Arrays (Part 2) <- You are here  
[Symmetric Difference of Multiple Arrays (Part 3)](http://www.vincecampanale.com/2017/01/05/symmetric-difference-of-multiple-arrays-part3/)  
[Symmetric Difference of Multiple Arrays (Part 4)](http://www.vincecampanale.com/2017/01/12/symmetric-difference-of-multiple-arrays-part4/)  

In this section, I'm going to go over steps one and two of the strategy outlined in the previous post.  
For a brief recap, in step 1, we want to separate each argument array and create a new array in our function to store them. In step 2, we will write a function that returns the symmetric difference of two arrays.

#### Step 1: Creating Arguments Array
I'm going to show two ways to do this. The first is an older, hackier way. The second uses `.from()` which was introduced in ES2015.

First way:  
~~~ javascript
var argArray = Array.prototype.slice.call(arguments);
~~~

This [Stackoverflow post](http://stackoverflow.com/questions/7056925/how-does-array-prototype-slice-call-work){:target="_blank"} explains this line of code very well.
Here's the most important information from that post:
>The `.call()` and `.apply()` methods let you manually set the value of this in a function. So if we set the value of this in `.slice()` to an array-like object, `.slice()` will just assume it's working with an Array, and will do its thing.

Second way:  
~~~ javascript
var argArray = Array.from(arguments);
~~~

This is a much cleaner way to get an array from an array-like object. We'll use the `.from()` approach in our final solution.

#### Step 2: Finding the symmetric difference of two arrays.
This step is definitely the hardest part of the solution. We *could* use a boat load of for loops here (which I spent a long time doing the first time I solved this), but I've recently discovered the beauty of higher-order functions. I'm going to skip straight to those because it's cleaner code.

Let's outline our function.

~~~ javascript
function symDiffTwoArrays(arr1, arr2) {
  //remove elements from array 1 that are in array 2
  //remove elements from array 2 that are in array 1
  //concatenate filtered arrays
  //return concatenated array
}
~~~

Okay, now we can use the [`Array.prototype.filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter){: target="_blank"} method to process each element in `arr1` and check if it is in `arr2`:

~~~ javascript
function symDiffTwoArrays(arr1, arr2) {
  //remove elements from arr2 that are in arr1
  var filteredArray1 = arr1.filter(function isInOtherArray(element){
    return arr2.indexOf(element) === -1;
  });
  //remove elements from arr2 that are in arr1
  //concatenate filtered arrays
  //return concatenated array
}
~~~

The method `.filter()` implements the callback function `isInOtherArray` on each element of `arr1` and only keeps elements for which the callback function returns true. *(Side note: You can use an anonymous function for the callback, as is most common, but I threw a name on there for the sake of clarity. (Side side note: Sometimes it's a good idea to give names to callback functions because it helps with understanding the call stack and debugging)).* In this callback, only elements that are not in `arr2` (i.e. their index strictly equals -1) will return true and thus remain in the `filteredArray1` array.

Then we give `arr2` the same treatment:

~~~ javascript
function symDiffTwoArrays(arr1, arr2) {
  //remove elements from arr2 that are in arr1
  var filteredArray1 = arr1.filter(function isInOtherArray(element){
    return arr2.indexOf(element) === -1;
  });
  //remove elements from arr2 that are in arr1
  var filteredArray2 = arr2.filter(function isInOtherArray(element){
    return arr1.indexOf(element) === -1;
  });
  //concatenate filtered arrays
  //return concatenated array
}
~~~

To keep our code [D.R.Y.](https://en.wikipedia.org/wiki/Don't_repeat_yourself){: target="_blank"}, we should separate the logic of the callback function out:

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
  //return concatenated array
}
~~~

Almost done!
Let's concatenate these arrays using [`Array.prototype.concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat){: target="_blank"} to get the symmetric difference and return that symmetric difference.

{% highlight javascript %}
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
{% endhighlight %}

Alright! We're done with most of the heavy lifting for this algorithm. We have processed the arguments passed to the function and written an inner, private function to calculate the symmetric difference of two arrays.

On to [Part 3]() to learn about `.reduce()` and how it works it's magic to help us bring this solution home!
