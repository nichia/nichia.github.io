---
layout: post
title:      "Campoutz – React/Redux Final Project"
date:       2019-06-19 17:03:22 -0400
permalink:  campoutz_react_redux_final_project
---

For my final project with Flatiron School, I created an app called Campoutz with React front end, using Redux to manage the state, and Ruby on Rails API back end. Campoutz fetches data from the Recreation Information Database (RIDB) API and displays the listing of campgrounds information. Users need to signup and login to save campgrounds to their favorites.

As expected, it was a tough project, and the process of working through the challenges helped in furthering my understanding of React and Redux.

Integrating JWT (JSON Web Tokens) authentication/verification process was one of the toughest parts of writing this application. JWT is a type of token-based authentication where for every single request from a client to the server, a token is passed for authentication, supporting the stateless API calls. Since I wanted to have user specific content, like saving favorites, or adding comments (still to be implemented), authentication process is a necessary requirement of this app.  

The use of Semantic UI components for styling the app was helpful, as the documentation was thorough, and there wasn’t much of a learning curve. For each styling feature that I implemented, I was able to easily follow the document and tailor it to my needs. Adding pagination for the list of campgrounds turned out easier than anticipated with Semantic UI.

The basics of Campoutz is done and deployed to Heroku. 


Click on the link to check out some campgrounds: [https://campoutz.herokuapp.com/](https://campoutz.herokuapp.com/)

<sub><sup>* Create your own login or use this pre-made account to save to favorites:  
    $ username: lorem  
    $ password: password</sup></sub>
 

