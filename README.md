# Sinatra Activerecord Setup

## TEACHER OUTLINE
This walkthrough should get students through the mechanics of adding activerecord to a sinatra application. Should mention that this generally isn't necessary as they'll usually start from a template with all of this set up, but it's god to understand the pieces.

should cover:
- activerecord gem
- creating rakefile with activerecord rake tasks
- configuring the database in environment.rb file

## Objectives

1). Understand the configuration pieces of using ActiveRecord with our Sinatra applications
2). Create and use a Rakefile to run ActiveRecord migrations
3). Setup a database connection

## Overview

Sinatra doesn't come with database support out of the box, but it's relatively easy to configure. In general, we'll be working from templates that have this pre-built, but it's good to understand what's going on under the hood. We're going to practice adding a database into our Sinatra applications. 

## Instructions

Fork and clone this repository to get started! We have a basic sinatra application stubbed out with an `app.rb` file acting at the controller.

### Adding Your Gems

First, we'll add two gems to setup ActiveRecord: `activerecord`, and `sinatra-activerecord`, `rake`. `activerecord` gives us the magical database mapping and interactions. `rake`, short for "ruby make", is a package that lets us quickly create files, folders, and automate tasks such as database creation. `sinatra-activerecord` gives us access to some awesome Rake tasks. 

```ruby
 	gem 'sinatra'
	gem 'activerecord'
	gem 'sinatra-activerecord'
	gem 'rake'
	gem 'thin'
	gem 'require_all'
```

Into our development group, we'll add two other gems: `sqlite3` and `tux`. `sqlite3` is our database adapter gem - it's what allows our Ruby application to communicate with a SQL database. `tux` will give us an interactive console that pre-loads our database and ActiveRecord relationships for us. Since we won't use either of these in production, we put them in our `:development` group - this way, they won't get installed on our server when we deploy our application.

```ruby
 	gem 'sinatra'
	gem 'activerecord'
	gem 'sinatra-activerecord'
	gem 'thin'
	gem 'require_all'
	
	group :development do
		gem 'shotgun'
		gem 'pry'
		gem 'tux'
		gem 'sqlite3'
	end
```

Our Gemfile is up to date - awesome! Go ahead and run `bundle install` to get your system up to speed.

### Making a Rakefile

As we mentioned, `rake` gives us the ability to quickly make files and setup automated tasks. We define these in something called a Rakefile. First, create a file at the root of the directory called `Rakefile`. In your `Rakefile`, we'll require our `config/environment.rb` file to load up our environment, as well as `"sinatra/activerecord/rake"` to get Rake tasks from the `sinatra-activerecord` gem.

```ruby
require'./config/environment'
require "sinatra/activerecord/rake"
```

In the terminal, type `rake -T` to view all of your available rake tasks. You should see the following output:

```bash
rake db:create              # Creates the database from DATABASE_URL or config/database.yml for...
rake db:create_migration    # Create a migration (parameters: NAME, VERSION)
rake db:drop                # Drops the database from DATABASE_URL or config/database.yml for t...
rake db:fixtures:load       # Load fixtures into the current environment's database
rake db:migrate             # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:status      # Display status of migrations
rake db:rollback            # Rolls the schema back to the previous version (specify steps w/ S...
rake db:schema:cache:clear  # Clear a db/schema_cache.dump file
rake db:schema:cache:dump   # Create a db/schema_cache.dump file
rake db:schema:dump         # Create a db/schema.rb file that is portable against any DB suppor...
rake db:schema:load         # Load a schema.rb file into the database
rake db:seed                # Load the seed data from db/seeds.rb
rake db:setup               # Create the database, load the schema, and initialize with the see...
rake db:structure:dump      # Dump the database structure to db/structure.sql
rake db:structure:load      # Recreate the databases from the structure.sql file
rake db:version             # Retrieves the current schema version number
```

