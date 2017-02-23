---
layout: post
title:  "Using the Vignere Cipher to Encrypt a Message (Part 1)"
date:   2017-01-20
---

This post is Part 1 of a three part series:
Using the Vignere Cipher to Encrypt a Message (Part 1) <-- You are here
[Using the Vignere Cipher to Encrypt a Message (Part 2)](http://www.vincecampanale.com/blog/2017/02/01/vigenere-cipher-part2/)
[Using the Vignere Cipher to Encrypt a Message (Part 3)](http://www.vincecampanale.com/blog/2017/02/06/vignere-cipher-part3/)


The Vign&egrave;re Cipher is a "method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of the keyword." Read more about it [here](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher), it has a really cool history. It's called a "polyalphabetic cipher" because it uses two or more cipher alphabets to encrypt the original message.

In the next couple of posts, I'm going to walk through my solution to implementing a Vign&egrave;re Cipher in Javascript. I'm sure there are plenty of ways to do it, and I'll probably find this solution pretty basic when I look at it again in a year, but for now, I'm satisfied. I thought this problem was a lot of fun to solve and was a great extension of the Caesar Cipher, which I covered in my [last post](http://www.vincecampanale.com/blog/2017/01/20/caesar-cipher/).

### How it Works  
The process goes like this:  

**Step 1** Get the message you want to encrypt (no spaces, all lower case), for example "thisisamessage."  
```
message: thisisamessage
```  

**Step 2** Choose a keyword to encrypt the message with, i.e. "lemon", and repeat it over and over until you get a string that is the same length as your message.
```
keyword: lemonlemonlemo
```  

**Step 3** This step is very similar to the Caesar Cipher. Add each letter of the original message to the letter of the keyword string to produce a new letter. For example, `t` is index 19, and `l` is index 11. If you combine them, and wrap back around to the beginning of the alphabet, you end up with the letter `e` at index 4. After you do that for every letter, you'll get the cipher text. In this example,
```
ciphertext: elugvdeysfdess
```

**Alternative Step 3** Instead of combining the indices of the letters in the message string and the keyword string, you can actually use a Vign&egrave;re table to look up the new letter. This is a bit harder to implement in code, but it's a fun exercise for sure.

I'll go through both solutions in this series. I'll cover the Caesar Cipher solution first since it's a bit easier in my opinion.

Continue to [Part 2](http://www.vincecampanale.com/blog/2017/02/01/vigenere-cipher-part2/) to see the solution without the table or skip to [Part 3]() to see the solution with the table.
