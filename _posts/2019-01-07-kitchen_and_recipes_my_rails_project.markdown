---
layout: post
title:      "Kitchen&Recipes: My rails project"
date:       2019-01-07 00:34:22 -0500
permalink:  kitchen_and_recipes_my_rails_project
---

Kitchenandrecipes is a web application that manages and shares recipes.

The application is built using Ruby on Rails framework and it manages data through forms and RESTful routes. It uses devise gem to provide standard user authentication for signup, login, logout and passwords. In addition, the authentication system allows login from third party services such as Facebook, Google and Github implemented via omniauth gem. Cocoon is a great gem utilized to handle the nested forms so that users can input all the recipe information in one form.

For the front-end, I decided to use Twitter Bootstrap 4 gem which allows the use of 'card' to display images of the recipes.

I ended up upgrading Ruby as Ruby version 2.5.0 includes the method to redirect http to https when accessing user avatar image from facebook.

Kitchena&Recipes accepts and manages the user avatars and recipe images, so Rails 5.2.1 active storage feature is timely in handleing the file uploads and storage of these avatars and images.

What I gained most from this project is that I may not know how to implement all the features of my project, but the internet is a wealth of information, and the anwers is out there, just a search away!





