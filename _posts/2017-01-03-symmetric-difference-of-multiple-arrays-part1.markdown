---
layout: post
title:  "Solved: Symmetric Difference of Multiple Arrays (Part 1)"
date:   2017-01-03
url:  http://www.vincecampanale.com/blog/2017/01/03/symmetric-difference-of-multiple-arrays-part1/
---
This post is Part 1 of a four part series.  
Symmetric Difference of Multiple Arrays (Part 1) <- You are here  
[Symmetric Difference of Multiple Arrays (Part 2)](http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part2/)  
[Symmetric Difference of Multiple Arrays (Part 3)](http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part3/)  
[Symmetric Difference of Multiple Arrays (Part 4)](http://www.vincecampanale.com/blog/2017/01/12/symmetric-difference-of-multiple-arrays-part4/)  

After countless failed attempts and lines of code deleted, I finally solved the problem of finding the symmetric difference of multiple arrays. If you're struggling with this problem for homework, Free Code Camp, Codewars, or wherever you may have encountered it, rest assured that you are not alone -- this one is a doozy. The solution has several distinct steps to it and in the interest of keeping these posts reasonably concise, I've decided to split the explanation of this problem into several parts.

In the first part, I'm going to give a little background information and talk about the overall strategy to solving this problem.

#### Background Info

The symmetric difference between two sets is best defined as the collection of elements which are members of either set, but not both. For example, the symmetric difference of `A = {1, 2, 3, 4}` and `B = {3, 4, 5, 6}` is `A ∆ B = {1, 2, 5, 6}`.

Things get a little harrier when we add in a third set (and a fourth and a fifth and ...) because the symmetric difference of all of the sets includes the unique elements *and* the elements that are present in *all* of the sets. To illustrate, let's add the set `C = {2, 4, 6, 8}` to the above example. To find the symmetric difference of `A = {1, 2, 3, 4}`, `B = {3, 4, 5, 6}`, and `C = {2, 4, 6, 8}`, we will first get the symmetric difference of `A` and `B`, which is `A ∆ B = {1, 2, 5, 6}`. Then, we take the symmetric difference of `A ∆ B` and `C` like so,
`(A ∆ B) ∆ C = {1, 4, 5, 8}`. Notice that the number 4 is in all three sets, so it is present in the symmetric difference.

#### Strategy

The general strategy for solving this problem relies on the `reduce()` method for array-like objects in Javascript, which I will go into more detail about in Part 3. Here's the outline:

1) Store all the arguments (which will be arrays) to the function in an array so we can use `.reduce()`.

2) Write a function that finds the symmetric difference between two arrays. We will use this function as the callback function for `.reduce()` to get the symmetric difference of all the arrays.

3) Write a function that removes duplicates from an array.

4) Sort and remove duplicates from the result of the symmetric difference function.

5) Put it all together.

Now that you have an idea of how this solution works, it might be a good time to give this problem another shot on your own. Continue to [Part 2](http://www.vincecampanale.com/blog/2017/01/05/symmetric-difference-of-multiple-arrays-part2/) to see steps 1 and 2 implemented.
