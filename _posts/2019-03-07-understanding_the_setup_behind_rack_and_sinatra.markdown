---
layout: post
title:      "Understanding the Setup Behind Rack and Sinatra "
date:       2019-03-07 09:19:13 -0500
permalink:  understanding_the_setup_behind_rack_and_sinatra
---

Sinatra is a DSL (domain specific language) for Ruby that is used to develop light-weight web applications. Sinatra utilizes Rack as its middleware. Rack is also a Ruby gem that allows us to write small bits of code and compile them to form a complex web application and when paired with Sinatra acts as a bridge between our Ruby application and our database. When running Sinatra on Rack, you will need to have a `config.ru` file that looks something like this:

```
require 'sinatra'
 
require_relative './app.rb'
 
run Application
```

`config.ru` loads the application environment, code, and libraries. Let's break this down a bit. `require 'sinatra'` specifically allows us to load our Sinatra library, `require_relative './app.rb'` requires the application file, located in `app.rb` and `run Application` uses the command `run` to boot up the Application as defined in `app.rb`. Let's head to that file now! Our Application class should inherit from Sinatra::Base, this is our primary controller. By inheriting from Sinatra::Base our app gains a Rack-compatible interface by using the Sinatra framework. This controller defines all URL requests and responses of the application using Sinatra's DSL.

Now let's take a look at our `environment.rb` file. Below you will see a basic example:

```
ENV['SINATRA_ENV'] ||= "development"

require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)

require './app/controllers/application_controller'
require_all 'app'
```


* `ENV['SINATRA_ENV'] ||= "development"` tells our application which environment we're working in, such as development, test or production. 
* `require 'bundler/setup'` ensures that we require Bundler, which includes all the defined gems in our gemfile and sets the load paths.
* `Bundler.require(:default, ENV['SINATRA_ENV'])` loads Bundler, automatically requiring all dependencies in our gemfile so that we don't have to require each one manually. In this case the requirement is conditional, depending upon the environment.
* `ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)` connects our application to our database using the Active Record gem, which is an object relational mapper utilizing SQL (in this case sqlite3) as the adapter and setting the database in the appropriate environment. Active Record provides us all of our CRUD methods along with hundereds of others and provides a DSL to easily query our database using SQL.
* `require './app/controllers/application_controller'` requires our application controller file which includes the Application Controller class.
* `require_all 'app'` requires our application to load all of the files in the application, which includes the models, views and controller(s).




