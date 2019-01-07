---
layout: post
title:      "Kitchen&Recipes: My rails project"
date:       2019-01-07 05:34:21 +0000
permalink:  kitchen_and_recipes_my_rails_project
---

Kitchenandrecipes is a web application that manages and shares recipes.

The application is built using Ruby on Rails framework and manages data through forms and RESTful routes. It uses the Devise gem to provides standard user authentication, including signup, login, logout and passwords. In addition, the authentication system allows login from third party services, including Facebook, Google and Twitter via omniauth gem. Cocoon gem is utilized to handle the nested forms so that users can input all the recipe information in one form, including ingredients and instructions.

For the front-end, I decided to use Twitter Bootstrap 4 gem which allows the use of card to display images of the recipes. I ended up upgrading to Ruby 2.5.0 as this version provides the fix to redirect http to https to access the user avatar image from facebook.

Kitchena&Recipes stores user avatars and recipe images. I use Rails active storage to handle the file uploads and storage of the avatars and images.

What I gained most from this project is that I may not know how to implement all the features of my project, but the internet if a wealth of information, and the anwers is out there, just a search away!





