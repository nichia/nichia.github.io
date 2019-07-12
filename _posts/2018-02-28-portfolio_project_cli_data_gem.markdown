---
layout: post
title:      "Portfolio Project: CLI Data Gem"
date:       2018-02-28 13:47:14 -0500
permalink:  portfolio_project_cli_data_gem
---


Book-Releases is my first portfolio project for the Full Stack Web Development program.  

Book-Releases is a CLI Ruby Gem that lists upcoming book releases by "Barnes and Noble" and "Books a Million". This gem provides a CLI (Command Line Interface) for user to choose their preferred store to get a list of the upcoming book releases as well as a detailed information of the book.

The project seemed overwhelming at first. Where would I start? The only thing I knew at the start of this project was that I wanted it to be closely related to books, as I enjoy reading. 

Not sure how to start, I watched all of Avi's video related to this project. As Avi suggested, I outlined the interface and noted down what I wanted of the application first. To create the Ruby Gem, I used bundler, which sets up the architecture of the application, ensuring that the files and directories layout are appropriate.

The application uses Open-uri and Nokogiri (both are Ruby Gems) to access and scrape the Barnes & Noble and Books a Million websites for information on upcoming book releases. The use of Pry to debug in real time was instrumental in extracting the required HTML and CSS data from the webpages.

During the project, there were many days lost when I could not fix a program bug. But through the use of the Pry debugger, things eventually moved forward. Never lose hope!

This CLI gem is on [RubyGems](https://rubygems.org/gems/book-releases)
- To install the gem, type ```gem install book-releases``` on the command prompt
- To run the application, type ```book-releases```

Book-Releases code is on [github](https://github.com/nichia/book_releases_cli_app).

