---
layout: post
title: "deleting crontab system mail"
date: 2013-04-01 21:17
comments: true
categories: 
---

***Last week, I presented an app to <a href="http://www.meetup.com/nyc-on-rails/">NYC on Rails</a>.*** The main focus was to learn about ActionMailer, paired with cron jobs and the <a href="https://github.com/javan/whenever">Whenever gem</a>.

***The hardest part of all of it was configuring the cron log*** to write to the correct files. I was getting permission errors when trying to get cron to write to the log directly in my application. In the end, I managed to configure the crontab to print errors to a directy of my computer.
    
    set :cron_log, "~/log/cron_log.log"

***While this gave cron proper permissions*** and allowed me to debug during development, I ended up 283 system emails on my computer, sent directly to victoria@victoria-miranda-friedman's-macbook. And I had no idea how to remove them from my root directory. 

Every time I entered terminal, I saw this message: 

{% img /images/post_images/youve_got_mail.png %}


In order to enter your mailbox, type `mail`. This will display:

{% img /images/post_images/mail_display.png %}

The return in terminal clearly tells you `Type ? for help.`, which returns a list of all possible methods to act on your mailbox and its contents

{% img /images/post_images/mail_options.png %}

And from here, you get clear instructions on how to delete your mail. To delete one email, enter `d 1`. If you wanted to delete ten emails, you would enter `d 1-10`, or in my first attempt `d 1-283`. 

{% img /images/post_images/delete.png %}

The next thing I learned about cron mail, was that `exit`, exits you from your inbox, but doesn't save any changes. `quit` is the command that saves and exits. And as you can see in the image above, after I `quit` and then entered my mailbox again, I'm told `no mail for victoria`










