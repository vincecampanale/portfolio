---
layout: post
title:  "Solved: Symmetric Difference of Multiple Arrays (Part 3)"
date:   2017-01-05
url: http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part3/
---
This post is Part 3 of a four part series:  
[Symmetric Difference of Multiple Arrays (Part 1)](http://www.vincecampanale.com/2017/01/03/symmetric-difference-of-multiple-arrays-part1/)  
[Symmetric Difference of Multiple Arrays (Part 2)](http://www.vincecampanale.com/2017/01/05/symmetric-difference-of-multiple-arrays-part2/)  
Symmetric Difference of Multiple Arrays (Part 3) <-- You are here  
[Symmetric Difference of Multiple Arrays (Part 4)](http://www.vincecampanale.com/2017/01/12/symmetric-difference-of-multiple-arrays-part4/)  

In this post, I focus on the third step of the solution: write a function that removes the duplicates from an array.

The function we want will take an array as an argument and filter out duplicates. Seems like a good time to use `.filter()`, eh?

There are other solutions for this which involves for loops, but in my opinion, a solution that makes use of a higher order function like `.filter()` is generally more concise, readable, and elegant.

Without further ado:
~~~ javascript
function removeDuplicates(arr) {
  return arr.filter(function(item, pos) {
    return arr.indexOf(item) == pos;
  });
}
~~~

Our function will step through every element in the array and filter out any element that has an index which does not match it's `.indexOf(element)` value. Since the `.indexOf(element)` method always returns the index of the *first appearance* of that element in the array, any element that appears more than once in the array will be removed.

That's it for Part 3!

In [Part 4](), I'll walk you through applying `.reduce()` to the arguments array using our symmetric difference helper function, then sorting and filtering the results (removing duplicates).
