---
layout: post
title:      "Campoutz - Heroku Deployment"
date:       2019-06-23 12:20:51 -0400
permalink:  campoutz_-_heroku_deployment
---

Now that Campoutz app is completed and running locally, it is time to look into sharing the app on the internet. I decided to stay on familiar ground and deploy my first web application on Heroku, like many of my fellow Flatiron School classmates.

These two blog posts, [A Rock Solid, Modern Web Stack—Rails 5 API + ActiveAdmin + Create React App on Heroku](https://blog.heroku.com/a-rock-solid-modern-web-stack) and [ReactJS + Ruby on Rails API + Heroku App](https://medium.com/@bruno_boehm/reactjs-ruby-on-rails-api-heroku-app-2645c93f0814) were really helpful in explaining  the steps for deploying web stack applications with ReactJs frontend and Ruby on Rails API backend on Heroku. After reading these blog posts and the articles provided by [Heroku](https://devcenter.heroku.com/), I was able to host Campoutz app on Heroku.
## Foreman
Without Foreman, in order to run Campoutz locally, we would use two terminal windows, one to execute the Rails API server `$ rails s -p 3001` and the other to execute webpac dev server `$ npm start`

To simplify the boot up, we will install Foreman to run the app with a single command.
### Set up Foreman:
Install Foreman gem: `$ gem install foreman`

Add Foreman to our `gemfile` (in :development group):
```
group :development, :test do
  # Boostrap RSpec
  gem 'rspec-rails', '~> 3.8'
  # Add support for Capybara system testing
  gem 'capybara'
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: %i[mri mingw x64_mingw]
  # Use Foreman to manage multiple processes
  gem 'foreman', '~> 0.82.0'
end
```
	
Update our gem: `$ bundle install`

Create a `Profile.dev` file in root directory (we’ll only use this for dev since we don’t need a node server in production) to contain:
```
web: cd client && PORT=3000 npm start
api: PORT=3001 && bundle exec rails s
```

With Foreman set up to manage multiple processes, we can run Campoutz with just the terminal command: `$ foreman start -f Procfile.dev`

Let's also create a rake file `start.rake` in our lib/tasks directory to let us run the app with the rake command: `$ rake start`
```
namespace :start do
  desc 'Start development server'
  task :development do
    exec 'foreman start -f Procfile.dev'
  end

  desc 'Start production server'
  task :production do
    exec 'NPM_CONFIG_PRODUCTION=true npm run postinstall && foreman start'
  end
end

task :start => 'start:development'
```
## React Router
To handle different pages within Create React App (using React Router) when the app is deployed on Heroku, we will require changes to the Rails app.

First tell Rails to pass any HTML requests that it doesn’t catch (not listed on *config/routes* file) to our Create React App. 

Then change the *app/controllers/application_controller.rb* from using the *API* to using *Base*.

In the *app/controllers/application_controller.rb*, add a *fallback_index_html* method:
```
class ApplicationController < ActionController::Base

  def fallback_index_html
    render :file => 'public/index.html'
  end
end
```

Then add *get* route at the bottom of the *config/routes.rb* file:
```
get '*path', to: "application#fallback_index_html", constraints: ->(request) do
  !request.xhr? && request.format.html?
End
```

That way, Rails will pass anything it doesn’t match over to the *client/index.html* so that React Router can take over.

Next, create a new `api_controller.rb`  file under the *app/controllers* directory:
```
class ApiController < ActionController::API
end
```

Change any new controllers you make to inherit from *ApiController*, not *ApplicationController*.  This ensures that any controllers you make can continue to take advantage of the slimmed down API version. For example:
```
class UserController < ApiController
end
```
## Environment Variables: Create-React-App & Dotenv
Dotenv is a module that loads variables from a *.env* file into *process.env* global variables when deployed. It's used for storing keys, URL’s and other sensitive information.

Install dotenv module: `$ npm install --save dotenv `

Next, require the module from *client/src/Index.js* or *client/src/App.js*  by adding the below line:
```
require('dotenv').config()
```

Create  a `.env`  file in the root directory of our client project. Then we can add each environment specific variable on a new line:  `REACT_APP_SECRET=VALUE`
```
REACT_APP_API_RIDB_ENDPOINT=https://ridb.recreation.gov
REACT_APP_RIDB_API_KEY=123456a7-1234-1234-1234-12b123c0ab12
REACT_APP_GOOGLE_MAPS_API_KEY=AbcdStYasI1wnj6wWCHWn3-4HR9gHKBG
```

We can now access the keys and values defined in our  *.env* file. For example:
```
const RIDB_URL = `${process.env.REACT_APP_API_RIDB_ENDPOINT}/api/v1`;
const RIDB_API_KEY = process.env.REACT_APP_RIDB_API_KEY;
```

Later, when creating Heroku app, we can set up each of the environment variables using terminal command `$ heroku config:set`.

Remember to add the .env file to your .gitignore so it doesn’t get saved to GitHub

## *package.json*: for node.js
The Campoutz app to be deployed on Heroku is a node.js application, so we need to create a *package.json* file in the root directory for our application. This *package.json* file is different from the package.json file that was created for the client react app.

Create `package.json` file in the root directory with this command: `$ npm init` and follow the set up prompts.

Then update the file to include the script values:
```
"scripts": {
      "build": "cd client && npm install && npm run build && cd ..",
    	"deploy": "cp -a client/build/. public/",
    	"postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  	}
```
## *Procfile*: for production
Next create a new `Procfile` in our root directory to tell production how to start the rails app (note: this was a step set up in rake start:production earlier on). 

Add the below line to *Procfile*.
```
web: bundle exec rails s 
```
## Heroku: Deployment
Create an account on [Heroku website]( https://signup.heroku.com/login).

Install [Heroku CLI]( https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

Login to Heroku from the root directory of your project: `$ heroku login`	
### Heroku: Create app
Create your app on Heroku either on Heroku website or from the terminal using Heroku CLI: `$ heroku apps:create campoutz`

Confirm a remote named `heroku` has been set for your app: `$ git remote -v`

If your app was created using Heroku website, add a remote to your local repository with: `$ heroku git:remote -a campoutz`
### Heroku: Buildpack
Let’s now tell Heroku to start by building the node app using *package.json*, and then build the rails app with the following terminal commands:  
`$ heroku buildpacks:add heroku/nodejs --index 1`  
`$ heroku buildpacks:add heroku/ruby --index 2`

[Buildpacks]( https://devcenter.heroku.com/articles/buildpacks) will tell Heroku that we want to use two build processes, first use node to manage the front end (requires *package.json*), and then ruby for the Rails API (requires *gemfile*).

We can now test our production build locally with our Rake task `$ rake start:production`. This rake task will run *foreman* (with our new *Procfile*) and kickstart the rails server, that gets it's scripts from the /public folder (that previously only contained a robot.txt file). Open a browser to view the local app at `localhost:5000`

Remember to add the /public folder to your .gitignore so it doesn’t get saved to GitHub
### Heroku: Environment variables
Next, configure each of the environment variable either from Heroku website or using Heroku CLI command: `$ heroku config:set REACT_APP_API_RIDB_ENDPOINT=https://ridb.recreation.gov
`

Run the above command for each of the environment variables in your *Client .env* file. 

If your Rails app uses *dotenv* gem to manage Rails environment variables, remember to run the above command for each of these variables too.

If your Rails app uses *figaro* gem instead of *dotenv*, run the figaro command to set values from your Rails configuration file all at once:  
`$ figaro heroku:set -e production`

You can run `$ heroku config` to see a list of the configured environment variables available on Heroku.
### Heroku: Deploying code
Now we are ready to [push the production app out to Heroku](https://devcenter.heroku.com/articles/git) with these terminal commands:  
`$ git add .`  
``$ git commit -m "ready for first push to heroku"``  
`$ git push heroku master`

After the app is built on Heroku, set up the database and seed file :  
`$ heroku run rake db:migrate`  
`$ heroku run rake db:seed`

Finally,  celebrate the success of deploying your application:  
`$ heroku open`
## Conclusion
That completes the deployment of Campoutz app to the internet.

You can find my app here: [https://campoutz.herokuapp.com/](https://campoutz.herokuapp.com/) 

<sub><sup>* Create your own login or use this pre-made account to save to favorites:  
    $ username: lorem  
    $ password: password</sup></sub>
## Sources
[A Rock Solid, Modern Web Stack—Rails 5 API + ActiveAdmin + Create React App on Heroku](https://blog.heroku.com/a-rock-solid-modern-web-stack)

[ReactJS + Ruby on Rails API + Heroku App](https://medium.com/@bruno_boehm/reactjs-ruby-on-rails-api-heroku-app-2645c93f0814)

[Buildpacks]( https://devcenter.heroku.com/articles/buildpacks)

[The Procfile](https://devcenter.heroku.com/articles/procfile)

[SQLite on Heroku](https://devcenter.heroku.com/articles/sqlite3)

[Deploying with Git](https://devcenter.heroku.com/articles/git)

[Heroku Postgres](https://devcenter.heroku.com/articles/heroku-postgresql#provisioning-heroku-postgres)

[Configuration and Config Vars](https://devcenter.heroku.com/articles/config-vars)

