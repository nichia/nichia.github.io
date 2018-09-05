---
layout: post
title:      "GEM’ming GoodreadsBooks"
date:       2018-09-03 20:16:45 -0400
permalink:  gem_ming_goodreadsbooks
---


Introducing the GoodreadsBooks CLI gem!

This command line application lists books that are Winners of Goodreads Choice Awards. By default, the app will list the choice awards winning books from the most recent year as published by [Goodreads](https://www.goodreads.com/choiceawards).  

Users have the option to select a different Choice Awards year to get a list of the winning books from that chosen year. The awards year available for accessing (scraping) is from 2010 to the most recent year, which is usually the previous calendar year. From the list, users have the option to choose a book and get detailed information about their selected book. In addition, users can select to open a link to Goodreads website to read the reviews & information about that book.

This is the second Ruby Gem that I’ve built for the CLI Data Gem Project assessment. Having completed the first application early February, I figured it’ll probably take me a while to peruse and refactor the first application to function better… (hint: the first Upcoming Book Releases data gem is slow… it needs to be refactored…). Might as well build another gem, keeping in mind the performance issue that I might face in scraping the data. 

It’s so much easier to build the CLI gem a second time. Initially, I planned on coding the user interface prompts and use ```heredoc``` to simulate the data. However, once I started and after examining the html codes and identifying the data for scraping, I decided to change my approach and get the data scraping methods working first.

When testing the completed GoodreadsBooks CLI gem, I realized there’s a performance issue. It takes around 60 seconds to scrape the 20 winning books from year 2017. This performance issue will surface each time users select a new awards year, which calls the Nokogiri parser to access and scrape the best book of each category data from Goodreads web page . For each category’s best book, Nokogiri parser is called again to access and scrape detailed information of that book (2nd level of html page). The advantage in getting the detailed information of the best books upfront, is that GoodreadsBooks will efficiently display the book information each time users select a best book for viewing.

A trade off is to fix the slow upfront loading time when users select a new awards year, and having a slower display time when users select a new best book for detailed viewing. I went with this approach. To change the code, refactoring was needed to only scrape each detailed book information when users select that book for viewing. 

This modification sped up the initial loading time to just under 10 seconds each time a new awards year is selected. When users then select a best book for detailed viewing, if the book details have not been scraped from Goodreads web page (i.e. first time viewing this book), Nokogiri parser is called to access and scrape this book's details, resulting in a slower screen display when this function is performed. 

Overall, I’m pleased with the improved performance of the GoodreadsBooks CLI gem. I hope you are too. 

GoodreadsBooks CLI gem is on [RubyGems](https://rubygems.org/gems/goodreads-books)
- To install the gem, type ```gem install goodreads-books``` on the command prompt
- To run the application, type ```goodreads-books```

GoodreadsBooks code is on [github](https://github.com/nichia/goodreads_books).
