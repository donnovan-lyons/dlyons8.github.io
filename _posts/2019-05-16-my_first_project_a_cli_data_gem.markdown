---
layout: post
title:      "My First Project: A CLI Data Gem"
date:       2019-05-16 19:38:31 -0300
permalink:  my_first_project_a_cli_data_gem
---

### Can you help me to watch a movie?

For my first project, I built out an application that provides a Command Line Interface (CLI) to a public website, and used that as the source for all the data in my app. How?

Scraping. What's scraping you may ask? 

Wikipedia defines it in the following way: "Web scraping, web harvesting, or web data extraction is data scraping used for extracting data from websites." Basically, what that means is that through writing code, I'm able to take almost anything that I see on a website, encapsulate into class objects (via object oriented programming), and feed that to my application which organizes it and churns it back out to the user in an organized and useful way, i.e. through my CLI!

The problem with creating such a program however, is that by writing code to scrape data from a website, I can run into bugs later on if that website changes, because the code that I use to scrape the data may no longer be applicable to the redesigned website, and I'd have no choice to but rewrite the code I use to scrape.

This article walks you through my thought process, some of the tools I used, and how the code works.

After a brief browse, I decided on using the website [A Good Movie To Watch](https://agoodmovietowatch.com/). It's a website with a movie database curated by actual people, and it provides users with the option of selecting their movie randomly, by mood, best films,  latest suggestions, and more. A clicking of any of the links to these categories on the website would then result in a page of one or more suggestions.

Any project requires planning, and so I planned according to the following steps:

----------------

1. Imagine interface.
2. Start with project structure.
3. Start with the entry point - the file run.
4. Force that to build the CLI interface.
5. Stub out the interface.
6. Start making things real.
7. Discover objects.
8. Program.

### Imagining the Interface

The interface that I wanted was one in which a user wants a movie, loads up our app, and then is presented with three options: 

1. Random
2. Best Films
3. Latest Suggestions

Option 1 would allow a user to randomly select a movie, one at a time. Options 2 and 3 would create a drop down list where a user can select a movie based on the appeal of the title, and obtain more information about it.

### Project Structure

The process to creating a ruby gem is made really simple. I began my process by perusing the [Developing a RubyGem using Bundler guide](https://bundler.io/v1.13/guides/creating_gem) and decided on using Bundler, because it makes gem dependency management and the accomplishment of publishing my gem to RubyGems easier.

After checking what version of Bundler I had, let the gem creations begin!

`bundle gem movie-helper`

The beautiful thing about Bundler, is that it creates for you the Gemfile, Rakefile, Code of Conduct, license, Gem specification file, and more.

There would be three classes: CLI, Movie, and Scraper. The CLI class would contain all of the logic necessary to properly navigate through the application while managing the interaction between the Scraper and Movie classes. The Movie class would be the objects that are instantiated from the data collected by the Scraper class.

### The File Run

Wanting the bin file to be nice and short, the key here was to simply invoke a CLI method that would contain all of the CLI logic for a fully functional app. In this instance, `.call`.

### The CLI Interface

What I typically do when creating my CLI interfaces is, I create my `.call` method by dividing up each concern that I have. I want my app to

1. Greet the User
2. Scrape the Data
3. Provide interactive menu
4. Close program

Each of these were divided into their own respective methods. `greeting` would be the method that welcomed the user simply by printing strings.  `load` would obtain all of the information inside of the app through scraping. `list_options` would print the menu for the user to interact with and capture any inputs. `goodbye` is another method that thanked the user for using their app.

### Stubbing Out the Interface

To make certain the CLI interface would work as planned, I created dummy data in the `list_options` method. The idea here was to fake the data, and get a more fleshed out interface only to replace the fake data with real data sourced from the website itself.

### Making Things Real

I knew immediately the `nokogiri` and `pry` gems would be necessary, so they were placed in the gemspec file.

Nokogiri is what makes scraping a reality, as it parses out the HTML documents and makes them searchable via CSS3 selectors. Thus, the essential and most important part here was to use Nokogiri to extract the data from the HTML and its embedded nodes.

Therefore by obtaining the URL of a particular movie, with Nokogiri one should be able to extract the: title, year, watch_with, watch_when, genre, review, stars in movie, movie rating, movie language, and url.

### Discover Objects

Each of the bits of data scraped for each movie were then placed into a hash. A hash would be used to initialized a movie, and each key would equate to each attribute in order to create full objects.

### Program

The final part of creating this CLI was replacing the dummy data used to flesh out the interface, and ensuring that the methods that we needed to initialize a Movie, and display its attributes were created.

See the final product in action below:

[Movie Helper Walkthrough](https://youtu.be/GWNkktgsH7E)

You can also download this published gem at [RubyGems.org](https://rubygems.org/gems/movie_helper)!
