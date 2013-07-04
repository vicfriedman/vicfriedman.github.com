---
layout: post
title: "action mailer configurations"
date: 2013-03-24 16:26
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
This week, I started working on a presentation with two other girls, <a href="http://1aurabrown.github.com/">Laura Brown</a> and <a href="http://anabecker.tumblr.com/">Ana Becker</a> to present at the <a href="http://www.meetup.com/nyc-on-rails/">NYC on Rails</a> meetup this coming Thursday (March 28). Our app revolves around using <a href ="http://rubygems.org/gems/actionmailer"> ActionMailer</a>. And this was the first time I've tried to use the gem. 

The documentation for it is really good, and the implementation doesn't require too many steps. <a href="http://guides.rubyonrails.org/action_mailer_basics.html">RailsGuides</a> has a great step by step breakdown of the entire process, inluding the steps for generating a Mailer.

All of that was easy. I only wanted to render a default template in my view, and am staying away from user authentication.

My problem fell in the configurations. I had no idea that I had to setup SMTP configurations to send my email. And then once I figured that out, where do I enter my specific configurations.

**Step 1:** <br>
At first we weren't getting any errors. It seemed as if our app was sending emails...but no one was getting any of them. The first thing we had to do was edit `config/evironments/development.rb`. That file contains the line:
      config.action_mailer.raise_delivery_errors = false
which needs to be changed to read:
      config.action_mailer.raise_delivery_errors = true
And then we started to get errors we could work from.

**Step 2:** <br>
Because we're using a Gmail account for our gem, we had to configure ActionMailer specifically for that account. The first step to that was to include the following two lines:
    config.action_mailer.perform_deliveries = true
    config.action_mailer.delivery_method = :smtp
These lines delcares that SMTP will deliver our emails. SMTP stands for Simple Mail Transfer Protocol. This protocol also has a layer of authentication built into it's functionality

**Step 3:**<br>
The next step is to declare your specific SMTP configurations.
     config.action_mailer.smtp_settings = {
      :address => 'smtp.gmail.com',
      :port => 587,
      :domain => 'localhost:3000',
      :authentication => :plain,
      :user_name => '<email_address>',
      :password => '<email_password>',
      :authentication => 'plain',
      :enable_starttls_auto => true
    }
These configurations state that we're using Gmail, we're running our app on localhost:3000, and also includes the email address and password for that account that are associated with the app. The last two lines of the settings handle the authentication of the email account. 

And that's it! 




