# hacker-news-posts  

Project using CSV Reader, list slicing, and various functions to analyze a CSV dataset in Python

# Project Details:  
Hacker News is a site where users can submit or vote on posts about technology and computer science, similiar to Reddit. Hacker News is extremely popular in technology and startup circles, and posts that make it to the 'front page' of Hacker News generate tens of, if not thousands of views.

There are two types of posts we will be focusing on in this project, Ask HN and Show HN posts.
Ask HN: The user submits a question in which other users can provided a response or vote on their favorite response.
Show HN: The user does not have a question but wants to show something ineresting, such as a project or an article

We'll specifically compare these two types of posts to determine the following:

Do Ask HN or Show HN receive more comments on average?
Do posts created at a certain time receive more comments on average?

It should be noted that the data set we're working with was reduced from almost 300,000 rows to approximately 20,000 rows by removing all submissions that did not receive any comments, and then randomly sampling from the remaining submissions.
