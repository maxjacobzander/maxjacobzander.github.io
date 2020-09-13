---
layout: post
title:      "My First App"
date:       2020-09-13 23:27:56 +0000
permalink:  my_first_app
---

## An Adventure in CLI's, Scraping, and Cocktails

The first time I came across the guidelines for the CLI project, it felt almost unattainable. In the short span of about a week, we would be creating our own CLI apps from scratch, either by utilizing API’s or by scraping an existing website. But hell, API’s were still such a new concept to me and actually scraping a website seemed so difficult. What’s more, I barely knew what a CLI even was! And building an actual, functioning application?! That seemed like a huge undertaking for someone who, just a short time ago, was taking the first baby steps of a journey into coding and programming. Writing this post on the other side of it all, all I can say is “Wow! What a week!”

So anyway, what is a CLI? Well, it may not be what most people would think of as an “app” in the year 2020, but a CLI (or Command Line Interface) is a text-based program that allows the user to view and interact with files through typed commands. In creating my first CLI application, I knew that I wanted to build something practical. I had to find a subject to build it around that would be interesting for users and could be informative. I decided to create a program that I now call “Cocktail Buddy.” The idea was that upon entering the program (and confirming that the user is of legal drinking age), the user would be presented with a menu of classic cocktails and, upon selection of a cocktail the program would return the ingredients and directions with which to make the drink. At the end of the returned recipe, the user would be presented with the option to either return back to the menu to select another recipe or exit the program.

At first, I thought I would accomplish this by finding an API that would fit my needs and gather my menu, ingredients, and instructions from there. An API (or Application Programming Interface) is a pre-existing set of structured data that, when properly utilized, can be used to transfer data from one source to another and using one definitely seemed like the quickest and easiest way to accomplish my goal. Upon further examination however, I realized that none of the API’s I found would quite suit my needs and it became clear to me that I would have to take a slightly more difficult route.

When a programmer scrapes a website, they are going into that website’s code to extract the data that they would like to use. Having spent a little bit of time teaching myself HTML, CSS, and JavaScript prior to starting up at Flatiron School, it was actually a little bit of a relief to realize that I would be working with a language that felt a little more familiar to me. I found a few websites that suited my needs and, upon inspection of the pages, found one that would scrape just fine!

In order to scrape this website in Ruby, I needed to require a few gems (aka packaged applications or libraries) including (but not limited to): “open-uri”, “nokogiri”, and “pry”. Whereas the “open-uri” gem has a rather obvious function, allowing us to open up a webpage like a file, the “pry” gem has taken me quite some time to come around to. “Pry” allows the programmer to pause the code in the middle of running it, to check and see how things are operating in the mid-run and to check to see how prospective code could work in the context of what is already written. The “nokogiri” (coming from the word for a Japanese fine-toothed saw) gem parses out the data from a website for us and makes it much easier to work with.

In order to most efficiently scrape my chosen site, I went and assigned the webpage’s nokogiri-parsed data to a variable within my “scraper” class. Then using “pry” and the CSS selectors that I had found while inspecting the page’s code, I was able to find the specific instances (index numbers, etc.) of the necessary data and then was able to assign those pieces of data to each of the attributes that I had given to my “cocktail” class. Having all of the necessary data be linked to each other through that class made this project exponentially easier, in that suddenly, I was working with data that was way more organized and could be interacted with in a much smoother and easier way.

While I know that this is still just the beginning of my developer development, I feel super excited to have something to show already that I’ve actually built. And, while it might sound a bit cliché, I also feel like there probably isn’t anything that is truly unattainable.

To try out Cocktail Buddy for yourself, please feel free to visit: https://github.com/maxjacobzander/cocktail-buddy
