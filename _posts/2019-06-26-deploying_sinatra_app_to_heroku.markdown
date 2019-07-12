---
layout: post
title:      "Deploying Sinatra App to Heroku "
date:       2019-06-26 21:32:53 -0400
permalink:  deploying_sinatra_app_to_heroku
---


Deploying the Sinatra app, My-Favetools, to Heroku is a simpler task than deploying the React/Rails app to Heroku.

## Postgresql
Heroku does not support Sqlite3 database. So the first order of business is to manually convert the sqlite3 database to Postgresql.

The steps to migrate to Postgresql are:

Update `Gemfile` to remove ``gem ‘sqlite3’`` and replace it with ``gem ‘pg’``

Run `$ bundle install`

Update `config.environment.rb` file to replace:  
```
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)
```  
with
```
set :database_file, './database.yml'
```

Create `config/database.yml` file to include:
```
development:
  adapter: postgresql
  encoding: unicode
  database: database_development
  pool: 2
production:
  adapter: postgresql
  encoding: unicode
  databaes: database_production
  pool: 5
```

Run `$ rake db:create` *(this creates the new database)*

Run `$ rake db:seed` *(this populates the database with seed data)*

## Procfile: for production
Add `Procfile` to the project root directory to declare what commands are run by the application’s dynos on Heroku platform.

```
web: bundle exec thin start -p $PORT
release: bundle exec rake db:migrate
```

Test on the local Heroku server, run `$ heroku local` and view the local app at `localhost:5000`

## Environment variables: dotenv
Update `Gemfile` to include:

```
gem 'dotenv'
```

Run `$ bundle install`

Update `config/environment.rb` to include:

```
require 'dotenv'
Dotenv.load
```

Create a `.env` file in the project root directory. Then we can add each environment specific variable on a new line: 
```
SESSION_SECRET=project_session_secret
```

From *application_controller.rb*, we can now access the secret defined in our .env file. 

```
set :session_secret, ENV['SESSION_SECRET']
```

Remember to add the .env file to your .gitignore so it doesn’t get saved to GitHub.
## Deploy to Heroku
Create an account on [Heroku website]( https://signup.heroku.com/login)

Install [Heroku CLI]( https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

Login to Heroku: `$ heroku login`

Create a Heroku app: `$ heroku apps:create my-favetools`

Configure the environment variables on Heroku:   
`$ heroku config:set SESSION_SECRET=project_session_secret`

Push your project to the new Heroku app: `$ git push heroku master`

After the app is built on Heroku, set the seed file if you have any: `$ heroku run rake db:seed`


Run `$ heroku open` to open the url in a browser and view your app

## Conclusion
That completes the deployment of my-favetools app to the internet. 

You can find my app here: [https://my-favetools.herokuapp.com/](https://my-favetools.herokuapp.com/)

<sub><sup>*Sign up and login to keep track of and share your favorite tools. Or, you can use this pre-made user account setting to login:  
    $ username: jade  
    $ email: jade@email.com  
    $ password: Pass1@</sup></sub>
		
## Sources
[Deploying a Simple Sinatra App with Postgres to Heroku](https://medium.com/@dmccoy/deploying-a-simple-sinatra-app-with-postgres-to-heroku-c4a883d3f19e)

[Sinatra - postgresql](http://recipes.sinatrarb.com/p/databases/postgresql-activerecord?)

[Heroku - sqlite3](https://devcenter.heroku.com/articles/sqlite3)

[Heroku - rack base app](https://devcenter.heroku.com/articles/rack)

