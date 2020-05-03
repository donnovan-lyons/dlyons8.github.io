---
layout: post
title:      "The Sinatra Web Application"
date:       2019-06-03 22:23:31 -0300
permalink:  the_sinatra_web_application
---

Utilizing Sinatra for my first foray into building a web application was intimidating. There's the visualization of the ideal: what it is you want to obtain. Then there's the reality. That is, the limitations of what you can obtain in a reasonable amount of time within the framework of knowledge that you already have and can expand on. I for example, have a limited amount of CSS knowledge, and as much as I'd love to be able to create beautiful web applications, my goal was to create something functional and demonstrate my ability to build a solid web application.

### The Skeleton

Unlike my previous project, the CLI Data Gem, where I simply followed the instructions of [Developing a RubyGem using Bundler guide](https://bundler.io/v1.13/guides/creating_gem), and it created my skeleton and gem dependencies for me, Sinatra required that I build my skeleton and gem dependencies from scratch! Not only that, in preparation for learning Ruby on Rails, I also had to determine my MVC structure. That is, what are my relationships between my classes, and how can I implement them via Active Record? Active Record is only the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. There's also the V - views, that information that needs to be displayed to the user on the screen, and the C - controller, which responds to the user input and interacts with our data model objects and views.

So to begin, I created my project folder and inserted all of the files that are important to have a fully functioning web application:

`Gemfile` - contains our gems which sets us up with some tools for our project.

`config.ru` - provides the details for the Rack environment requirements of the application, and starts a server that's going to be able to respond to requests for applications built in this file.

`CODE_OF_CONDUCT.md`- entails standards community needs to uphold when working together on project contribution.
License.txt - document providing protection to contributers and users of a project.

`Rakefile` - a directory where Rake tasks are defined.

`README.md` - a file containing basic, crucial information about the software.

`app` - folder containing all files needed for MVC structure. This includes models, views, and controllers.

`config` - folder where database connection and required files for the application are established.

`db` - a folder containing all database files and their data.

## When building, why not build something that could be beneficial to your every day life?

I find it easiest, when taking on a project such as this, to visualize creating something that you would actually deem useful in your everyday life. This enabled me to not only code my application based on theory, but to modify as I build through actual utilization of the application as I run into error or user based need/feature. Currently, I'm studing in what's called a yeshiva, where most of my day to day tasks include studying Jewish law and translating the text. Here, the idea that I came up with was to build a website where I could create a dictionary that I could copy and paste a section of Hebrew and Aramaic words into, that would then be parsed through to delete any repetitive words owned by that User, construct a table, and then I could translate each word in that table one by one.

### Filling Everything Out

The `Gemfile` is essential, as it contains all of the files you'll need to employ the desired tools. For this application, that meant `sinatra`, `sqlite3`, `activerecord`,`rake`,`pry`,`sinatra-activerecord`,`shotgun`,`bcrypt`, and `rack-flash3`. This enabled me to have everything essential for my database, testing, running a server, encrypting my password, and more.

I edited my `config/environment.rb` file to setup my database connection and require all the gems in my gemfile like so

```
require 'bundler'

Bundler.require

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/development.sqlite"
)
require_all 'app'
```

 Then the config.ru file was edited to mount my controllers and load my environment.
 
 ```
 `require 'bundler'

Bundler.require

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/development.sqlite"
)
require_all 'app'
```

### The Database

I decided for my MVC structure, my Models would contain a User, Words, Tables, and Tractates. The Controllers would be each of these plus a Sessions controller, and the Views would contain the visible HTML pages as needed, e.g. show, edit, index pages etc. As I go along, I would add or edit out what was needed to further build out my application.

The relationships needed to be built. I knew that a User had many Words, Tables, and Tractates. A table had many words and belonged to a Tractate. The Tractates had many Users, and many Tables. After some time, I realized that Tables would be the perfect join table for Users and Tractates, as a User only had many Tractates through Tables, and a Tractates had many users through Tables. These relationships were added to the Model classes, and then the Rakefile was filled out to load the environment and require necessary files for the needed tasks:

```
require_relative './config/environment'

require 'sinatra/activerecord/rake'

task :console do
  Pry.start
end
```

Tables were then created, edited, and migrated for the proper Model class relations and their methods.

### All Creativity From Here

After all the above steps are taken care of, the Controllers and Views are next. The ApplicationController itself should have cookies enabled and any other needed static folders set, such as `public` and `views` so that Sinatra knows where to find them. The other controllers manage the route's actions, and the views will display whatever HTML is inside of them once routed there.

Any CSS and JS files should be added to a `public` folder that you put into your main directory. In Sinatra it's the default directory where such static files should be served from.

Take a look at a video walkthrough of my project in action: [Gemara Dictionary](https://youtu.be/KBymCwTrjoM)!

