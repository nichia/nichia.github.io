---
layout: post
title:      "Campoutz – React/Redux Final Project"
date:       2019-06-19 17:03:22 -0400
permalink:  campoutz_react_redux_final_project
---

For my final project with Flatiron School, I created an app called Campoutz with React front end, using Redux to manage the state, and Ruby on Rails API back end. Campoutz fetches data from the Recreation Information Database (RIDB) API and displays the listing of campgrounds information. User need to signup and login to save campgrounds to their favorites.

As expected, it was a tough project and the process of working through the challenges helped in further my understanding of React and Redux.

Integrating JWT (JSON Web Tokens) authentication/verification process would have to be one of the toughest part of writing this application. JWT is a type of token-based authentication where for every single request from a client to the server, a token is passed for authentication, supporting the stateless API calls. Since I wanted to have user specific content, like saving favorites, or adding comments (still to be implemented), authentication process is a necessary requirement of this app.  

The use of Semantic UI components for styling the app was really helpful as the documentation was thorough, and there wasn’t much of a learning curve. For each styling feature that I implemented, I was able to easily follow the document and tailor it to my needs. Adding pagination for the list of campgrounds turned out easier than anticipated with Semantic UI.

The basics of Campoutz is done and deployed to heroku. 


Click on the link to check out some campgrounds: [https://campoutz.herokuapp.com/](https://campoutz.herokuapp.com/)


<sub><sup>* create your own sign-up or use this pre-made account to save to favorites:  ' username: lorem and password: password </sup></sub>
 

