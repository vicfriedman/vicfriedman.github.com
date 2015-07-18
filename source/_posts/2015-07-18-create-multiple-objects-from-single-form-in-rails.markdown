---
layout: post
title: "Create Multiple Objects From Single Form in Rails"
date: 2015-07-18 15:57
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

**I recently wanted to to create at least 10 Answer objects from a single form.** Essentially, I was building a quiz app and wanted people to be able to take a quiz. I finally figured it out, but had to back out to a much simpler sample to fully understand what was going on. So I made an app to create a bunch of puppies.

I generated a new rails app with a single model,`Puppy`. To start, I knew I wanted to create a form that would eventually look like this: 


{% img /images/new_puppy_form.png %}

##Step 1:

First, I needed to change my `new` controller action to create 5 new puppies, instead of just one.

** puppies_controller.rb **
```ruby
  def new
    @kennel = []
    5.times do
      @kennel << Puppy.new
    end
  end

```

Each new puppy that I create `Puppy.new` is stored in an array  named `@kennel`. This way, later in my create action, I can iterate over `@kennel` to save each puppy to the database.

## Step 2:

This also meant that my form needed to be structured slightly differently. I need the `params` to contain an array of each individual puppy information. I wanted to structure my form such that the `params` hash looked like this:

```ruby
  params = {
    "puppies" = [
      {"name" => "Joe", "breed" => "Pomeranian"}, 
      {"name" => "Victoria", "breed" => "Mastif"}
    ] 
  }

```

** views/puppies/new.html.erb **

```html
<%= form_tag puppies_path do %>
<% @kennel.each do |puppy| %>
  <% end %>
  <div class="actions">
    <%= submit_tag %>
  </div>
<% end %>
```

I started out with a form tag, that submits a `POST` request to the Puppy `create` action. Next, I need to take my array of puppies, which I named `@kennel` in my `new` controller action and iterate over that array to generate fields for each specific puppy in that array. This way I have one single form with many sets of fields.

Now that I have a way to access each instance of the `Puppy` class that I'm creating, I need to start generating form fields for each attribute. In this case, I only have two attributes, name and breed.

```html
<%= form_tag puppies_path do %>
  <% @kennel.each do |puppy| %>
    <%= fields_for 'puppies[]', puppy do |p| %>
  
      <div class="field">
        <%= p.label :name %><br>
        <%= p.text_field :name %>
      </div>
      <div class="field">
        <%= p.label :breed %><br>
        <%= p.text_field :breed %>
      </div>
      
    <% end %>
  <% end %>
  <div class="actions">
    <%= submit_tag %>
  </div>
<% end %>
```


I set up ` <%= fields_for 'puppies[]', puppy do |p| %>` so that I can set up the params hash to contain an array of puppies. Basically I'm setting that there will be a key in the params hash called `puppies`. From there, I set up the text_fields for name and breed.

## Step 3:

But this means that I can't use strong parameters as they are normally used. Strong parameters would expect a key called `name` and a key called `breed` inside of a key named `puppy`. 

```ruby
  params = {
    "puppy" => { 
      "name" => "Joe", 
      "breed" => "pomeranian" 
    }
  }
```


Instead, it would find those keys inside of a hash, inside of an array, which is the value of the key `puppies`, which is exactly how I started out trying to design the `params`:


```ruby
  params = {
    "puppies" = [
      {"name" => "Joe", "breed" => "Pomeranian"}, 
      {"name" => "Victoria", "breed" => "Mastif"}
    ] 
  }

```

** puppies_controller.rb **

```ruby
  def puppy_params(my_params)
    my_params.permit(:name, :breed)
  end
```

I set up my `puppy_params` method to accept an argument, which would be some hash containing the parameters by which I would want to create each instance of my `Puppy` class. Inside the method, I use the `my_params` argument and permit those keys (name and breed) to pass through to the `Puppy` model.

## Step 4:

This means I can't simply call `Puppy.create(puppy_params)` anymore...

** puppies_controller.rb**

```ruby
def create
  params["puppies"].each do |puppy|
    if puppy["name"] != "" || puppy["breed"] != ""
      Puppy.create(puppy_params(puppy))
    end
  end

  end
```

Instead, I have to iterate over the value of the key `puppies` in the params hash. From there, I have the specific hash for each individual puppy. To create a puppy now, I have to call `Puppy.create(puppy_params(puppy))`. I also had to set up a check to make sure the `name` and `breed` keys are not storing empty strings (in the event that someone wanted to only make 3 puppies and not 5).

## Step 5: 

But this also means that my app breaks if I have a form to just create one single puppy.


** application_controller.rb **

```ruby

def create
    if params.has_key?("puppy")
      Puppy.create(puppy_params(params["puppy"]))
    else 
      params["puppies"].each do |puppy|
        if puppy["name"] != "" || puppy["breed"] != ""
          Puppy.create(puppy_params(puppy))
        end
      end
    end
  end
```

I had to set up an if statement to check if the params has the key `"puppy"`, which is how information is passed back to the controller from the form to create a single instance of the puppy class. If params did in fact have that key, then I have to create an instance of the puppy class by calling `Puppy.create(puppy_params(params["puppy"]))`.Remember, I set up the `puppy_params` method to accept an argument. I couldn't just pass params because I took the `.require(:puppy)` out of the strong params. This method no longer knows how to look inside a hash to find the desired data, it needs to be passed the specific hash. I passed in `params["puppy"]` as the argument to the puppy_params method.

And finally, I can create 1 puppy, 10 puppies, or 1 billion puppies.


















