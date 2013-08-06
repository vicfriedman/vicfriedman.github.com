---
layout: post
title: "How To Write Rspec Mailer Tests"
date: 2013-08-05 20:17
comments: true
categories: 
---

***The last few weeks have been a whirl-wind of building, which has been so wonderful.*** I really feel like I got to flex my muscles a little bit (I learned to encrypt things..post to come later) but the other side of building is testing. I built three different mailers, and then learned that you can actually test your mailers with rspec. And it's not that difficult! 

__I chose a few very basic pieces of mailer functionality to test:__<br>
1. Was it actually sending the email<br>
2. Was the email being sent to the desired recipient<br>
3. Was the subject line of the email correct<br>
4. Was the email being sent from the correct address

__This was also really helpful because up until this week, we've only been able to test mailers locally.__ These Rspec tests were essentially the only way I was able to tell if the mailers were functioning how I wanted them to function.

So here was my mailer method: 

    def confirmation_email(ordered_story)
      mail(to: story.email, subject: "Here Is Your Story!", content_type: "text/html")
    end

__In my mailer spec, the first thing I did was set up a few configurations__ for ActionMailer that I declared to happened before each test.

      before(:each) do
        ActionMailer::Base.delivery_method = :test
        ActionMailer::Base.perform_deliveries = true
        ActionMailer::Base.deliveries = []
        @story = Factory.create(:story)
        EbookConfirmationMailer.confirmation_email(@story).deliver
      end


I'm basically saying: send my emails in the testing environment (which does have to be explicitly declared), please actually perform all my deliveries, create an array of all my sent emails, build the factory for my story class (with an instance of that class), and then finally deliver my email.

__Then, I wanted to clear all of that after each test so I have a clean slate before each test.__

    after(:each) do
      ActionMailer::Base.deliveries.clear
    end

__So now that I have all of that set up, the tests.__

__1. Is the email being sent?__
    
      it 'should send an email' do
        ActionMailer::Base.deliveries.count.should == 1
      end

I had originally set up my array of delivered emails, so in this test, there should only be one email. My before statement only called one email to be delivered.

__2. Is the email being sent to the desired recipient?__
  
      it 'renders the receiver email' do
        ActionMailer::Base.deliveries.first.to.should == @story.email
      end

The Story class, has an email attribute, which is what is declared as the `to:` in my mailer method. The Story Factory builds an instance of the class with an email address which is the one that should be used to send the email (@story.email).


__3. Does the email have the correct subject line?__

      it 'should set the subject to the correct subject' do
        ActionMailer::Base.deliveries.first.subject.should == 'Here Is Your Story!'
      end

The subject line of the email was expplicitly defined in my mailer. So if my email is sending properly, that subject line should be rendered in the email. 

__4. Was the email being sent from the correct addres?__
    
      it 'renders the sender email' do  
        ActionMailer::Base.deliveries.first.from.should == ['notifications@stories.com']
      end

When I defined my mailer, I defined a default from address `default :from => "notifications@stories.com"`
So just like the subject line, if the email is being sent properly, that should be the from address, plain and simple.

I guess the next step with these will be testing the mailer views (I also recently learned that you can write tests for views! Cool!)..so clearly I have lots more blog posts to write.
