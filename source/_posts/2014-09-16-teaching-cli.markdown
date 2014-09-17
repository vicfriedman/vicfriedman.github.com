---
layout: post
title: "Teaching CLI"
date: 2014-09-16 11:04
comments: true
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
***The Nuts and Bolts:***
__About six months ago__ I joined <a href="https://flatironschool.com">Flatiron School</a> to start something really awesome...a program to teach high school students to code. After teaching over 100 students this summer during six two-week camps and the preparations for our [after school programs](https://after.flatironschool.com/) I've learned a lot about teaching and coding.


***CLI:***

When I learned command line, it felt like a necessary evil. This thing I had to learn in order to get to the cool stuff. I wanted to build web apps that people would use and think were awesome. But before that, I had to learn CLI and git and sql and all this *stuff*. That's like 3 languages before you even get to the good stuff.

At Flatiron School, we don't teach coding in contrived environments. Not for our adult immersives, and definitely not for our high school programs. We want give our students the ability to use real tools to build real things. Which means, again...teaching our students to use terminal like the pros. But it sort of seems to suck.

***How It's Taught Now***

The best guide available online is Zed Shaw's [Learn CLI the Hard Way](http://cli.learncodethehardway.org/book/).It's actually a great tutorial to master the commands. It's how I learned. But let's think about it for a second. A class of 20 fifteen-year-olds will eat you alive if you hand them that and say "type 'mkdir director_name' 10 times, followed by 'rm -r directory_name'". Especially on day one! 

Our students have never coded before. Never even knew that terminal existed on their computer. Didn't know that that was life before the GUI. Didn't even know what a GUI was. So you can't just give them meaningless exercises to learn this potentially really boring thing.

The single most frequently asked question in every class by most every student is "Why are we learning this? What's the point?"

***What Is The Point?!*** 

Command Line provides an incredibly easy interface to your computer. You don't spend seconds looking for your lost mouse, clicking icons, waiting for finder to load, waiting for files to load. Typing `open Desktop/Development/my_file.rb` takes milliseconds! As a developer, your time is expensive, and those seconds you waste navigating through finder add up to minutes and then hours that you waste trying to find and open files. That's why we claim the best developers never have to touch a mouse.


***So How Do You Make It Interesting??***

First and foremost, terminal can't be treated as the boring thing. It has to be cool in and of itself.

Here are some points I've tried to emphasize:

**1.** IT'S NOT SCARY!! You cannot explode your computer into a million pieces just by typing in a bad command. Just bang on the keyboard and press enter. It'll be ok I promise.

**2.** A themed story makes a world of difference. I've used the story of saving a princess from a tower. You have to move through the territory ruled by the castle by creating the directories that make up the different areas of the territory as well as create files for the different people and creatures you meet along the way. This story allows students to repeat the commands over and over and over again, but not bore them to death with random pointless files. This way, they're on a mission.

**3.** `Say` command packs a big punch. You can even have the `say` command read from a file `say -f file_name`. You can change voices by passing the `-v` flag with an argument. It would look something like this `say -f file-name -v voice_name`. Apple provides Bruce, Kathy, Alex, Fred, Victoria, and Vicki as potential different voices. 

**4.** `rm -rf ` is THE MOST DANGEROUS. But it also make you feel super powerful that you can destroy a computer with four letters and a hyphen. 

**5.*** ALL THE TOYS. Just try typing `brew install cmatrix`. Once it installs enter `cmatrix` and install feel like the coolest.

***Aside*** You can even incorporate the `say` command into ruby code. Which is SUPER AWESOME.
```ruby
def say_hi(name)
  `say "hi #{name}"`
end
```