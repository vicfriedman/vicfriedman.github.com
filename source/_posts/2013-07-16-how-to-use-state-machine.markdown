---
layout: post
title: "How To Use State Machine"
date: 2013-07-16 20:11
comments: true
categories: 
---
**A few weeks ago, I was introduced to an incredible gem called State Machine** (you can read their docs <a href="http://slagyr.github.io/statemachine/documentation.html">here</a>). The gem keeps track of the status of a specific object and respeonds to different inputs to alter that state. The example that I've understood the most is in purchasing something (I'm going to use a pack of gum in this post).

__The gem breaks down into four different parts:__

1. State- the status of the object.
  All states are predefined in the class. All objects start in the initial state. The first step to buying a pack of gum is to select it. So the pack of gum would move from initial to selected.<br>

2. Transition- the movement from one state to another.
  A pack of gum would transition from the initial state to the selected, and then again transition to the queued state when the user is in line to purchase it.

3. Event- The invocation of a state. 
  A pack of gum would transition from the initial state to the selected, by way of an event. Another event would trigger the queued state.

4. Action- State Machine can trigger different methods before, during, and after transitions.


__So let's further elaborate on this example of buying a pack of gum.__

__What states would there be?__<br>
<li>Selected - I deciced I want zebra gum, so now I'm holding it</li>
<li>Queued - waiting in line to purchase</li>
<li>Purchased - handing money to cashier</li>
<li>Owned - I own and can do whatever I want with the gum</li>
<li>Trash- After I chew the gum, it's garbage</li>


__Here is an example of a class set up with State Machine,__ and then I'll go through and explain each step.
      class Gum
        attr_accessor :flavor, :price, :state
  
        state_machine :initial => :selected do
          
          before_transition :on => purchased :do => :demand_money
          after_transition :on => owned, :do => :be_chewed
          

          event :queue do
            transition :selected  => :queued_for_purchasing
          end

          event :purchase
            transition :queued_for_purchasing => :purchased
          end

          event :ownership
            transition :purchased => :owned
          end

          event :trash do
            transition :owned => :trash
          end

    
          state :queued do
            def is_queued?
              return true
            end
          end

          state :purchased do
            def is_purchased?
              return true
            end
          end

          state :owned do
            def is_owned?
              return true
            end
          end

          state :trash do
            validate { |gum| gum.trash? }
            def is_trash?
              return true
            end
          end

        end

        def demand_money
          #demand money here
        end

        def be_chewed
          # method to prepare piece of gum to be chewed
        end


      end


__So how does this work?__ 

State Machine first gets included in your Gemfile.

From there, the set up is all done from inside the class where the objects change states. All objects start in the `initial` state, which is defined with when State Machine is invoked:
      state_machine :initial => :selected do


__Next, the states must be defined:__
      
      state :queued do
        def is_queued?
          return true
        end
      end

      state :purchased do
        def is_purchased?
          return true
        end
      end

      state :owned do
        def is_owned?
          return true
        end
      end
      
      state :trash do
        validate { |gum| gum.trash? }
        def is_trash?
          return true
        end
      end

__Following, the events must be defined:__

      event :queue do
        transition :selected  => :queued_for_purchasing
      end

      event :purchase
        transition :queued_for_purchasing => :purchased
      end

      event :ownership
        transition :purchased => :owned
      end

      event :trash do
        transition :owned => :trash
      end

Here, the events specify transitions between states. These events can be triggered in the command line as well, which would trigger new states. The state is saved as an attribute of an instance of the class.

__Then, the actions to be called must be defined:__
       def demand_money
          #demand money here
        end

      def be_chewed
        # method to prepare piece of gum to be chewed
      end

__Finally, the actions must know to be called during transitions:__
      before_transition :on => purchased :do => :demand_money
      after_transition :on => owned, :do => :be_chewed


__So, to test the flow of all of this in command line,__ there is a handy method `fire_events`. That method looks like this:
      gum = Gum.first
      gum.state #=> selected
      gum.fire_events(:queue) #this transitions the gum from selected to queued_for_purchase
      gum.state #=> queued_for_purchase


The only thing to know is that you cannot rollback states through triggering events. You can however explictitely redefine the state (`gum.state = "selected"`) and from there retrigger the events.

Enjoy!




