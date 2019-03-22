---
layout: post
title:      "Rails with Javascript - Kitchen&Recipes"
date:       2019-03-22 10:53:40 -0400
permalink:  rails_with_javascript_-_kitchen_and_recipes
---


One would have thoght working off an existing project, adding new functionality, is an easier process than creating a brand new application. Not so, as I soon realized upon starting my fourth project for the Fullstack web developement program. 

The requirement of this project is to add dynamic Javascript front end features to my previous Ruby on Rails application, at the background, making ajax data interchanges with rails server via Json Api, without using rails "remote: true" function.

To keep with the theme of single page application, I reserved the root "/" url as the landing page for the dynamic javascript content display.  My intention is to maintain the original functionalities of the Rails application when accessing other pages of Kitchen&Recipes.

On starting Kitchen&Recipes, the application listens for the landing page window to display a list of recipes. Rails server-side pagination was added to handle the large number of recipe records so reading data from the DOM will not be unacceptly slow.

After loading all the recipes, the program listens for a click on any of the links available on the page (with the exceptions of links on the navigation bar which is still accessing the Rails function). 

Links available for users to click includes: 
- list my recipes or other user's recipes,
- pagination pages
- add/show/edit/delete a recipe, and
- add/edit/delete a review.

There were challenges that I faced when writting the application, and it's always so rewarding to learn so much in solving the issues each step of the way.


