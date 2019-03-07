---
layout: post
title:      "Understanding the Setup Behind Rack and Sinatra "
date:       2019-03-07 14:19:12 +0000
permalink:  understanding_the_setup_behind_rack_and_sinatra
---


Sinatra is DSL (domain specific language) for Ruby that is used to develop light-weight web applications. Sinatra utilizes Rack as its middleware. Rack is also a Ruby gem that allows us to write small bits of code and compile them to form a complex web application and when paired with Sinatra acts as a bridge between our Ruby application and our database. This requires three important parts and will be run whenever a request is received.

1. It must repond to a `call` method 
2. This `call` method utilizes one argument, typically `env` or environment, that bundles all of the components of the request
3. The `call` method must return the http status code, headers and the body. 

However, before we are able to run the `call` method we must first construct the HTTP web server to receive the requests and return the responses to the browser. This is done in the `config.ru` file using `rackup`. For example, your `config.ru` may look something like this and will be executed by running the command `rackup config.ru`:

```
#config.ru
require_relative "./application.rb"
 
run Application.new
```

This will allow you to load your site and develop it on your local machine without exposing it to the wild web. When running Sinatra on Rack, you will have a `config.ru` that looks something like this:

```
require 'sinatra'
 
require_relative './app.rb'
 
run Application
```

Let's break this down a bit. `config.ru` loads the application environment, code, and libraries. `require 'sinatra'` specifically allows us to load our Sinatra library, `require_relative './app.rb'` requires the application file, located in `app.rb` and `run Application` uses the command `run` to boot up the Application as defined in `app.rb`. Let's head to that file now! Our Application class should inherit from Sinatra::Base, this is our primary controller. By inheriting from Sinatra::Base our app gains a Rack-compatible interface by using the Sinatra framework. This controller defines all URL requests and responses of the application using Sinatra's DSL.

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
* `require 'bundler/setup'` ensures that we load the defined gems in our gemfile by defining the load paths
* `Bundler.require(:default, ENV['SINATRA_ENV'])` loads Bundler, setting the load paths and automatically requiring all dependencies in our gemfile so that we don't have to require each one manually. In this case the requirement is conditional depending upon the environment we are setting, such as development.
* `ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)` Connects our application to our database using the Active Record gem, which is an object relational mapper utilizing SQL (in this case sqlite3) as the adapter and sets the database using SQL dependent upon the environment. Active Record provides us all of our CRUD methods along with hundereds of others and provides a DSL to easily query our database using SQL.
* `require './app/controllers/application_controller'` requires our application controller file which incluedes the Application Controller class
* `require_all 'app'` requires our application to load all of the files in the application controller, which includes the models, views and controller(s).




