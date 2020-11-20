---
layout: post
title:      "Instance Variables: An Instance of a Useful Skill"
date:       2020-11-20 14:47:20 +0000
permalink:  instance_variables_an_instance_of_a_useful_skill
---


In learning about Rails and later creating my Rails project for Flatiron School, I knew that I would need to use a good deal of instance variables to successfully accomplish the development of my program. And whereas I did know what they are from my previous modules, working with Rails gave me a whole new appreciation for instance variables and what they do.

Well, to best explain what an instance variable is, I want to really quickly explain the concept of Object-Oriented Programming. In the shortest possible explanation I can give, Object-Oriented Programming (or OOP) is a way of writing code that utilizes classes to organize said code, and objects that we create from those classes (variables, methods, functions, etc.) in order to somewhat mimic the real world which, in many cases, makes our programming a bit clearer and a bit easier.   

**So, what exactly is an instance variable?**

 Well, an instance variable is object that we can use to assign information or attributes to our classes and keep track of that information. In Ruby, you’ll see it prepended with a `@` sign.

**So why use an instance variable?**

Unlike a local variable, which has a hard-and-fast definition that can only be used within the method the variable was created in, an instance variable can be thought as a bit more of a cookie-cutter or template of sorts. Rather than that very specific definition of an object, we are able to define the variable with something a little more broad so that we can apply it to various instances of an object. Just like in the real world, you may find yourself with more than one of a specific item. For example, maybe you have one phone charger, but maybe you have several, as to make sure that you always have one around. Using an instance variable would allow you to tell the code that when you say @charger at home, you want the one you keep plugged in by your bed, but when you say @charger in the car, you are referring to the one you keep there for when you're driving.

*(It is worth mentioning that this example would work if you had a class called `myplaces`. While I won't get too into the four big principles of OOP, one of them, Encapsulation, dictates that instance variables (and the other data belonging to an object) are not accessible outside of their classes (unless you give explicit permission to another class).)
*

Let's take a look at some instance variables in action. In my Rails application, Bucket, you would find the following in my bucket_lists_controller:

```
class BucketListsController < ApplicationController

    def new
        if user_signed_in?
            @bucketlist = BucketList.new
            @goal = @bucketlist.goals.build
        else
            redirect_to new_user_session_path
        end
    end

...

    def show
        @bucketlist = BucketList.find(params[:id])
    end
```



You’ll notice that the instance variable, `@bucketlist` has a different definition in each of the methods. In our `new` method, `@bucketlist` is defined as a brand new instance of a bucketlist, as opposed to in the `show` method, where it is helping us locate a specific instance that already exists.

Now let’s take a look at some views. If we look at the new page for bucket_lists, we’ll see:


```
<h2>Create a New Bucket</h2>
<br>
<%= form_with model: @bucketlist, local: true do |f| %>
    <%= f.label :name %><br>
    <%= f.text_field :name %><br><br>

    <%= f.fields_for :goals do |g| %>
    <%= g.label :name, "Add Your First Goal!" %><br>
    <%= g.text_field :name %>
    <% end %>

<br><br>
    <%= f.submit "Add Your Bucket" %>
<% end %>
```


Thanks to the instance variables used between the controller and the view, the `@bucketlist` used in our `form_with` will lead to the instantiation of a new BucketList object.  Now, if we go and visit the “show” page for bucket_lists, we will see the following:

```
<h2><b><%= @bucketlist.name %></b></h2>
<br>
<% if @bucketlist.goals != [] %>
    <h2><% @bucketlist.goals.incomplete.each do |goal| %>
        <%= link_to goal.name, bucket_list_goal_path(@bucketlist, goal) %><br><br>
        <% end %></h2>
        <% if @bucketlist.goals.accomplished.count >= 1 %><br>
            <h2><b>These Are The Goals You've Accomplished:</b><br><br>
            <% @bucketlist.goals.accomplished.each do |accomplishment| %>
            <%= link_to accomplishment.name, bucket_list_goal_path(@bucketlist, accomplishment) %><br><br>
            <% end %></h2>
        <% end %>
<% end %>
<br><br><br>
<h3><%= link_to "Add A New Goal", new_bucket_list_goal_path(@bucketlist) %></h3>
<br>
<h3><%= link_to "Edit Bucket", edit_bucket_list_path %> <%= link_to "Delete Bucket", bucket_list_path(@bucketlist), :method => :delete, :data => {:confirm => 'Are you sure?'} %></h3>
<br>
<h3><%= link_to "Back to Your Buckets", bucket_lists_path %></h3>
```

Take a look at that code.

You’ll see the `@bucketlist` instance variable all over it. What does that mean? Well, remember how, if we look back at the controller, `@bucketlist` is defined as the instance of BucketList that we specifically want to see this time ` (@bucketlist = BucketList.find(params[:id]))`. We are able to use that definition in the controller to call upon the attributes of the specific instance to provide us with the exact information we want from that instance. Looking a little lower in the code on the show page, we can even route our user to the proper place to add a nested attribute for this specific instance.

*N.B. routing the user to a join table path does require usage of the proper instance variable. For example, `newbucketlistgoalpath(@bucketlist)` routes the user to a different place than than `newbucketlistgoalpath(@goal)`. In the latter case, the user would be routed to URL: /bucketlists/:goalid/goal/new instead of the needed /bucketlists/:bucketlistid/goal/new and the new item would not be nested properly.*

Instance variables are a relatively simple and common tool to have in your coding utility belt, but do not underestimate its power. You can accomplish a lot using instance variables and I hope that reading this gave you a bit more of an understanding of what they are and the examples gave you a bit more of an understanding of how they work. Now go out there and use some instance variables!
