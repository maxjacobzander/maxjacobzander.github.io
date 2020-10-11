---
layout: post
title:      "Making Worlds Collide"
date:       2020-10-11 10:44:34 -0400
permalink:  making_worlds_collide
---


As a professional musician, I should have known from the first time I heard "Sinatra", that I would enjoy this unit of my time at Flatiron School. For those unfamiliar with the eponymous framework, Sinatra is designed to help build Ruby applications super quickly and without all of the frustration of building every last thing from scratch, while still leaving the application structure and communication to the developer themself. I realized while building my CLI app that I really enjoy building practical applications and, when tasked with building an MVC (Model-View-Controller) Sinatra application that was to track something important to me using CRUD (Create Read Update Destroy) functions, I realized that this would be a great opportunity to do just that.

Perhaps it was the musical name of the framework or perhaps it was the filming of an opera for a covid-era audience that I was doing during the process, but I decided to create an application to keep track of the musical scores in my personal library. To accomplish this, I had to create two tables using ActiveRecord migrations, one for users and one for the scores themselves, and build out my program from there. After all, a user has many scores and a score belongs to a user!

I designed the program so that a user could sign up for an account by using their email address and creating a password and, upon logging in to what I decided to call the "Bravo Personal Score Library Catalogue", the user can input any scores that they have acquired to keep a running list. While the program only requires the input of a title and a composer's last name (using validations), a user can input the work's title, the composer's first and last name, the genre (or sub-genre), and the year the work is from. Once created, the user can edit or delete the entry, view it with details on the "show page", or just view it by title and composer amongst the other scores in their catalogue on the index page.

Perhaps the greatest challenge that I had building out this program was deciding that I wanted a user to be able to view their catalogue and have the option of having the scores sorted by either title name or by composer name. Using a scope `scope :ordered, -> { order('title asc') }` I was able to define an ordered-by-title list as the default for use on my index page. Scopes are rather similar to class methods, but they guarantee an ActiveRecord relation and are super clean and concise. In order to give users the option for the composer view however, I needed to define a secondary Scope to use 
`scope :ordered_by_composer, -> { order('composer_last asc') }` and find a way to display it. I ended up creating a secondary index page that would iterate over each of the scores belonging to the `current_user` and have that list sort by the `ordered_by_composer` scope definition. Taking a hint from the code used to create a dropdown menu, I created anchor links on each of the index pages for each of the sort-by options. Should a user be on the desired page, that index's link won't go anywhere, but should the user want to change the view, the are able to click on the link for the other sort by option and the list display will change. Again, what is actually happening is a redirect to an identical page, with a different sorting during the iteration, but this allowed for me to accomplish exactly what I wanted within the catalogue.

Overall, this was a really fun experience and I have loved getting to know the Sinatra framework. I am super excited to continue working with it and to continue onto Rails and, even moreso, to continue building practical, functional applications!

Should you be a musician (or otherwise) interested in trying out or using the Bravo Personal Score Library Catalogue, you can do so by cloning the repo at: [https://github.com/maxjacobzander/bravo_score_library](http://)

(And for fun and good measure, here is a photo of me and my partner, Ashley with the scores for *Hansel and Gretel* (the aforementioned filmed opera) and in costume, on location as Hansel (Ashley) and the Witch (me)! Enjoy!)
![](https://drive.google.com/file/d/14mbCSf5uynrT1N54XnPzTh0fOkuMq4Of/view?usp=sharing)
