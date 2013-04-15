---
layout: post
title: "ruby marshaling"
date: 2013-04-10 14:11
comments: true
categories: 
---

***As my semester at <a href="http://flatironschool.com/">The Flatiron School</a> winds down,*** we've been spending our time building our final projects. My group, <a href="tmd.io">Tyler Davis</a>, <a href="dolin.github.com">Danny Olinsky</a>, and <a href="http://jlarusso.github.io/">Jesse La Russo</a>, is building a modern-take on an old fashioned gold star board. Aka, you write a blog post? You get a gold star. You finish a Code School course? You get a gold star. We've built in a lot of other functionality (our own authentication and authorization, the ability for users to give each other stars, background cron jobs, etc. etc.), but that's the gist of it.

***The best thing about this whole process*** has been the things I've learned from the errors we get. In this way, I learned about the <a href="http://ruby-doc.org/core-1.9.3/Marshal.html">Ruby Marshal Module</a>. 

***The Marshal Module*** essentially takes a collection of Ruby Objects and converts them into a byte stream, and then allows it to be stored and accessed elsewhere.


***One of this big things with our project has been working with APIs*** in order to automate the processes of giving stars. The Treehouse API was particularly difficult to parse through, so for the time being, I created a dictionary to deal with their objects. Essentially, we want to give a star for every category a user completes.
      @@dictionary = {
      "Website Basics" => ["Website Basics"],
      "Technology Foundations" => ['Graphic Basics'],
      "Aesthetic Foundations" => ["Elements", "Principles", "Color Theory"],
      "HTML" => ["Introduction", "Text", "Lists", "Links", "Objects", "Tables", "Forms"],
      "CSS Foundations" => ["Introduction", "Selectors", "Data Types", "Text", "Box Model", "Page Layout", "Printing", "Framework Foundations"],
      "CSS3" => ["Selectors", "Typography", "Web Fonts", "Borders", "Gradients", "Backgrounds", "Transitions", "Transforms", "Animation", "Multi-Column Layouts", "Media Queries"],
      "Build a Responsive Website" => ["Introduction to Response Web Design", "Fluid Foundation", "Adaptive Design", "Responsive Design", "Advanced Techniques"],
      "Introduction to Programming" => ["Basics", "Control Structures", "Objects and Arrays", "Functions"],
      "JavaScript Foundations" => ["Introduction Variables", "JavaScript Strings", "JavaScript Numbers", "JavaScript Arrays", "JavaScript Functions", "JavaScript Objects"],
      "Ruby Foundations" => ["Ruby Basics", "Objects, Classes, and Variables", "Ruby Strings", "Ruby Numbers", "Ruby Arrays", "Ruby Hashes", "Ruby Methods", "Ruby Loops", "Ruby Blocks", "Ruby Procs & Lambdas", "Ruby Modules", "Ruby Core", "Standard Library", "Ruby Testing"],
      "Build a Simple Ruby on Rails Application" => ["Getting Started with Rails", "Rails Frontend Development", "Ruby on Rails Authentication", "Customizing Ruby on Rails Forms", "Writing Tests", "Rails Routing", "Testing the Whole App", "Building the Profile Page", "Rails Deployment"]
    }

***In this dictionary,*** the star (which is also the Treehouse badge category) is the key. The values (represented as an array of strings) are all the badges you can earn. Essentially, the values are the requirements a user must fulfill to receive a Treehouse Star. In order to do that, we had to create methods that would parse through the Treehouse API, take a collection of all a user's badges, and iterate those badges through our dictionary.

      def check_star_requirement(th_json)
        #check json of badge names on profile
        badges = th_json["badges"].collect { |badge| badge["name"] }
        completed_stars_array = @@dictionary.each do |star, requirement|
          star if check_requirements(badges, requirement) 
        end
        completed_stars_array.compact
      end

      def check_requirements(badges, requirement)
        (badges & requirement) == requirement
      end 




The `check_star_requirement` methodtakes the Treehose json for a user (in my case, <a href="http://teamtreehouse.com/victoriafriedman.json">teamtreehouse.victoriafriedman.json</a>), iterates through their information on the Treehouse API and collects all their badges. Those badges are saved in an array called `badges`. Then the dictionary is saved as an array, `completed_stars_array` and iterated through.

It then calls the `check_requirements` method, which iterates through the values of the dictionary. If the strings in `badges` matches any of values in `dictionary`, then a user will receive that star.

All of this seemed to be working great until we checked our database. Rails "marshaled" our data. An individual entry in our Star table in the Name column, looked like this:

      Aesthetic Foundations
        - Elements
        - Principles
        - Color Theory

Avi pointed us towards the Ruby Marshaling Module. Essentially, Ruby was taking our entire collection, our dictionary, and transforming each key-value pair into a string (which apparently looked like the above example). We were essentially calling, `Marshal::Dump`, which saves the data and returns the original string. Because the `each` method also just returns the original string, our iteration wasn't just saving the key, `Aesthetic Foundations`, to our database, but the entire string. 

Our solution to this problem was really simple, essentially we replaced `each` with `collect`. Collect returns the new array, so in our case, it returns an array of key values in the dictionary, but only the key where values are all contained in the badges array. 





