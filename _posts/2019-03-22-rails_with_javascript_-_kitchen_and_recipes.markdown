---
layout: post
title:      "Rails with Javascript - Kitchen&Recipes"
date:       2019-03-22 10:53:40 -0400
permalink:  rails_with_javascript_-_kitchen_and_recipes
---


One would have thought that working off an existing project and adding new functionality would be an easier process than creating a brand new application. I soon realized this was not so, upon starting my fourth project for the Fullstack web developement program. 

The requirement of this project is to add dynamic Javascript front end features to my previous Ruby on Rails application. For the front-end to make ajax data interchanges with the back-end Rails server using JSON API, without utilizing rails "remote: true" function.

To keep the features of the original project, I reserved the root "/" url as the landing page for the dynamic javascript content display.  My intention was to maintain the original functionalities of the Rails application when accessing other pages of Kitchen&Recipes.

On starting Kitchen&Recipes, the application listens for the landing page window to display a list of recipes. Rails server-side pagination was added to handle the large number of recipe records, so reading data from the DOM would not be unacceptly slow.

After loading a page of the recipes, the program listens for a click event on any of the links available on the page (with the exceptions of links on the navigation bar which is still accessing the Rails function). 

Links available for users to click includes: 
- list my recipes or other user's recipes
- pagination pages
- add/show/edit/delete a recipe
- add/edit/delete a review

I faced many challenges when writing the application, but it has been rewarding to learn so much in solving these challenges.
