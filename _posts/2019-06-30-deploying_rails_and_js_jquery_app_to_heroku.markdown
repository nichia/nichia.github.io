---
layout: post
title:      "Deploying Rails and JS/jQuery App"
date:       2019-06-30 18:20:24 -0400
permalink:  deploying_rails_and_js_jquery_app_to_heroku
---

KitchenandRecipes is a Ruby on rails application (with javascript/jQuery), to manage and share recipes. It uses Rails 5.2 Active Storage to manage and save files to Amazon S3 cloud storage service.
```
App Configuration:

// ♥ ruby --version
ruby 2.5.0p0 (2017-12-25 revision 61468) [x86_64-darwin16]

// ♥ rails --version
Rails 5.2.3

// ♥ bundler --version
Bundler version 2.0.2
```
## Rails Asset Pipeline
The Rails asset pipeline provides an assets:precompile rake task to allow assets to be compiled and cached up front rather than compiled every time the app boots.
Compile Rails asset pipeline locally: `$ RAILS_ENV=production bundle exec rake assets:precompile`  
A public/assets directory will be created.

## Procfile: for production
Add `Procfile` to the project root directory to declare what command should be executed to start your app on Heroku platform.
```	
release: bundle exec rake db:migrate
```
Test on the local Heroku server, run `$ heroku local` and view the local app at `localhost:3000`

## Deploy to Heroku
Login to Heroku: `$ heroku login` 

Create a Heroku app: `$ heroku apps:create kitchenandrecipes` 

Configure the environment variable Heroku: example

``$ heroku config:set RAILS_MASTER_KEY=master.key``   
``$ heroku config:set FACEBOOK_KEY=fb_provider_key``

Push your project to the new Heroku app: `$ git push heroku master`

After the app is built on Heroku, set the seed file if you have any: `$ heroku run rake db:seed`

Run `$ heroku open` to open the url in a browser and view your app

## Major Issue
One major issue that I ran into - while the JS/jQuery pages are able to access the asset-pipelines data, the Rails views pages were not able to load as it could not access the image file from asset-pipeline. 
After searching, finally found the fix [on Stack Overflow]( https://stackoverflow.com/questions/49440304/rails-asset-is-not-present-in-asset-pipeline-when-using-image-tag
)

Update `config/environments/production.rb` file:  

```# config.assets.compile = true```
## Conclusion
That completes the deployment of kitchenandrecipes app to the internet. 

You can find my app here: [https://kitchenandrecipes.herokuapp.com/](https://kitchenandrecipes.herokuapp.com/)

<sub><sup>* Sign up and login to keep track of and share your recipes. Or, you can use this pre-made user account setting to login:
$ username: lorem
$ email: lorem@email.com
$ password: Password1!<sup></sub>
		

## Resource
[Container-Ready Rails 5](https://blog.heroku.com/container_ready_rails_5)
[Rails Asset Pipeline on Heroku ](https://devcenter.heroku.com/articles/rails-asset-pipeline)
[Getting Started on Heroku with Rails 5.x](https://devcenter.heroku.com/articles/getting-started-with-rails5)
[Getting Started on Heroku with Ruby](https://devcenter.heroku.com/articles/getting-started-with-ruby)

