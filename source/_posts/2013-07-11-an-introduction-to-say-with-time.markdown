---
layout: post
title: "An Introduction to say_with_time"
date: 2013-07-11 22:03
comments: true
categories: 
---

I don't think I will ever know everything about Rails. And that's part of its magic, every day there is something that makes me go "Woah, cool!"

Meet, `say_with_time`, a really handy little method used in migrations to create a more readable output. This is especially useful when your migrations are doing more than one simple task. I've seen it used during a migration that loads a yaml file. What? I didn't even know you could do that.

`say_with_time` is a method called directly in the body of a migration during the `up`, `down` or `change` methods. You must pass a block to the method, which controls the entire migration.

      def up
        say_with_time "adding columns email and deleted_at to profiles" do
          add_column :profiles, :email 
          add_column :profiles, :deleted_at
        end
      end

      def down
        say_with_time "removing columns email and deleted_at from profiles"
          remove_column :profiles, :email
          remove_column :profiles, :email
        end
      end


The output of running the migration will include the entire quote as well as a benchmark for when the migration was complete.

Easy and efficient way to control your output from migrations.

