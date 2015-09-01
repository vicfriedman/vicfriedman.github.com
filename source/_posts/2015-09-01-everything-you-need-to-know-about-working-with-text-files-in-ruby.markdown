---
layout: post
title: "Everything You Need To Know About Working With Text Files in Ruby"
date: 2015-09-01 13:37
comments: true
categories: 
---
<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-38989132-1']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>

 <img src="images/confused.gif" align="middle">

**When working with text files in my apps, I never seem to know when to read a file or when to just open it.** And once it's open how to I modify it from Ruby? I generally read through multiple different versions of docs, and end up going with my best bet (all while feeling equally as confused as Jennifer Lawrence). I figured there have to be rules around this..So here it is, consider this your cheat sheet to working with text files in Ruby.

## Opening A File

**In order to do anything with a file, you have to open it first.** Think about an document you receive in your email. Can you read or edit the document without opening it first? Nada.

Let's say my app file structure looks something like this:

```
|── app
    ├── lib
    │    └── my_code.rb
    |── fixtures
      ├── info.txt
      │── my_contents.txt
```

`lib/my_code.rb` is my ruby file where I will be writing my code to read from `fixtures/info.txt` and `fixtures/my_contents.txt`.

So let's say from inside of `lib/my_code.rb`, I wanted to open the contents of `fixtures/info.txt`:

```ruby
file = File.open('../fixtures/info.txt')
``` 

I now have the contents of the `info.txt` stored in a variable called `file`. It's important to note that the `file` variable is storing an instance of the `File` class. It's storing an actual File object. You can see what instance methods are available on this file by entering `file.methods`.

## Reading A File

**Now that I have my file open, it's time to read it.**

```ruby
file.read
```

Calling the `read` method on my File object will display the entire contents of the file as a single string. If the file were a CSV, you'd need to split the string based on a comma.

## Writing To A File

**In order to write to a file, you actually need to change the way in which you open it in the first place.** Let's say I'm going to write to `fixtures/my_contents.txt`:

```ruby

File.open('../fixtures/my_contents.txt', 'w') do |file|
  file.write("I am writing to a file from my program")
  file.close
end

```

This time, you have the pass the argument `'w'`, which is the flag to write to a file. Without this additional argument to the `open` method, you can't write to your file.

Next, inside the `do` block, you use the method `write` method. This method accepts an argument: the string you wish to write to your file. Finally, we call the `close` method, to close the opened file. And lastly, and `end` to close our block.



