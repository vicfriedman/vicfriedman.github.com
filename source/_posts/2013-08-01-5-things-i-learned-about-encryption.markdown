---
layout: post
title: "5 Things I Learned About Encryption"
date: 2013-08-01 19:49
comments: true
categories: 
---

***So I was recently asked to encrypt an id***, and then somehow take that encryption and display it as part of a URL. Essentially, it was a mock form of security for a new feature at work, so that a user couldn't just change an id in a url and move around things that should remain private.

_Encryption is an entirely new concept to me.__ I didn't even know what it really meant. So it was a really big learning exercise for me. Ps. Encryption means to convert data into cipher. Oh and cipher is "diguised way of writing code" (thanks <a href="https://www.google.com/search?q=cipher+definition&oq=cipher+definition&aqs=chrome.0.0l4.1710j0&sourceid=chrome&ie=UTF-8"> Google </a>).

__1. Ruby has an OpenSSL library used for encryption.__
This library has greate documention <a href="http://www.ruby-doc.org/stdlib-1.9.3/libdoc/openssl/rdoc/OpenSSL.html"> here </a>
