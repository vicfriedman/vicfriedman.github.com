---
layout: post
title: "sql indexes"
date: 2013-06-27 22:10
comments: true
categories: 
---

***In my last post, I mentioned that `unique` is a reserved word in SQL because of indexes.*** But to be totally honest, that was the first time I'd even heard of indexes. So I did some more research...and they're actually pretty cool.

__Basically, an index is a special lookup for tables in a database that are used__ to speed up data retrieval, aka quicker queries using `WHERE` and `SELECT` clauses. In even simpler terms, it's a pointer to specific data. Only downside istha it's slows down data inputs with `UPDATE` and `INSERT` clauses. Also, indexes can be created or dropped easily, and will have no effect on the code. 

__So first things first, you want to create an index. This process is incredible simple:__
      CREATE INDEX index_name
      on table_name (column_name);

__For example, I have a `recipes` table, and that table has the columns__ `name`, `ingredients`, and `preparations`. And say you want to be able to easily query for recipes with a specific ingredient, this would be a great time to use an index.
      CREATE INDEX recipe_ingredients
      on recipes (ingredients);

Super easy.

__Now, there are a few different kinds of indexes:__

__1. Unique Index: allows no duplicate values to be inserted__ in the selected column in your table.
      CREATE UNIQUE INDEX recipe_ingredients
      on recipes (ingredients);

It's simple to create a unique index, but there is an appropriate moment to use it. The above example is the proper way to create a unique index, but not it's proper use. You wouldn't be able to include butter in more than one recipe. Or salt. Or hot sauce. Not ok.


__2. Composite Index: used on two or more columns in a table.__

    CREATE INDEX recipes
    on recipes (ingredients, preparations);

This index will allow speedy retrieval of data in both the `ingredients` column and the `preparations` column. 


__3. Implicit Index: this is an implied index, that is automatically__ created with the formation of a PRIMARY_KEY column, or UNIQUE_KEY column. Any time your table has an ID column, you have an implicit index on that column.

Obviously, you need to know how to get rid of an index. It's also really simple and will not affect your data in any way.
      DROP INDEX index_name;
      DROP INDEX recipes;
      DROP INDEX recipe_ingredients;

Now we got rid of all the indexes (except the implicit index) on my recipes table. 


__So of course Rails has built in methods with ActiveRecord to deal with indexes.__ The declaration for indexes in rails is included in your migrations, but as the methods must become class methods....

    add_index :table_name, :column_names, options

Your options include `:name`, `:unique`, and `:order`

__So to take my same example of my recipes table from before__ and the migration AddIndexesToRecipes:

      def self.up
        add_index :recipes, ingredients, :name => 'recipe_ingredients'
      end  

In this example, I created an index on my recipes table, on the ingredients column, and I neamed the index "recipe_ingredients". If I had used :`unique`, I would have created the same problem as before, where my cookies could have butter, but my cake couldn't. No thanks.

__Rails also makes it really simple to remove indexes,__ and there are two ways to do so:

1. Remove an index by column name:
      remove_index (table_name, column: column_name)
      remove_index (recipes, column: ingredients)


2. Remove an index by index name:
      remove_index (table_name, name: index_name)
      remove_index (recipes, name: recipe_ingredients)



<ol>Helpful Resources:
  <li>http://www.tutorialspoint.com/sql/sql-indexes.htm</li>
  <li>http://api.rubyonrails.org/classes/ActiveRecord/Migration.html</li>
</ol>