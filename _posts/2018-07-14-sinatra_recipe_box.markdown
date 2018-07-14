---
layout: post
title:      "Sinatra Recipe Box"
date:       2018-07-14 19:59:41 +0000
permalink:  sinatra_recipe_box
---

Yes! another project completed... after some thoughts, I've decided to create a Recipe sharing system. 

Here's the steps taken to complete this project.

1. Create a new repository on GitHub for Sinatra Application name “sinatra-recipe-box”

2. Setup file structure (files and directories)

3. Create main files:

- README.md
- NOTE.md
- spec.md (copy from project assessment)
- LICENSE (created by github)
- config/environment.rb
- Gemfile then run bundle install to install any gems and gem dependencies for this application
- Rakefile
- config.ru

4. Create the db tables and models.
- create tables, type rake db:create_migration NAME=create_yourtablename. Create all the table.
- run rake db:migrate, and all your tables will be added

5. Setup associations for the tables /app/models/

6. Add controllers, routes and views

7. Models: Concerns - add slugifiable for recipe.name and user.name

8. Controllers: add Helpers

9. Add user login, signup and logout

10. Add category controller, routes and views

11. Add recipes routes and views

12. Add edit processing

13. Add delete processing

14. Add processing for Ingredients and Instructions

15. Add processing for Notes

That pretty much sums it up.
