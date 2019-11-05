# hacker-news-posts  

Project using CSV Reader, list slicing, and various functions to analyze a CSV dataset in Python

# Project Details:  
Hacker News is a site where users can submit or vote on posts about technology and computer science, similiar to Reddit. Hacker News is extremely popular in technology and startup circles, and posts that make it to the 'front page' of Hacker News generate tens of, if not thousands of views.

There are two types of posts we will be focusing on in this project, Ask HN and Show HN posts.
Ask HN: The user submits a question in which other users can provided a response or vote on their favorite response.
Show HN: The user does not have a question but wants to show something ineresting, such as a project or an article

We'll specifically compare these two types of posts to determine the following:

1. Do Ask HN or Show HN receive more comments on average?  
2. Do posts created at a certain time receive more comments on average?

It should be noted that the data set we're working with was reduced from almost 300,000 rows to approximately 20,000 rows by removing all submissions that did not receive any comments, and then randomly sampling from the remaining submissions. I've added the dataset CSV file to this repository.

# Opening and Exploring the Data:
Import the csv package

```python
import csv
```
# The Hacker News data set:
First we open the file and assign it to a variable(opened_file). Afterwards, we can use the Reader function within the CSV package to appoint the file as a CSV. Lastly, we can just close the the open() link.

Note: If you run into an error named UnicodeDecodeError, add encoding="utf8" to the open() function.
Example: open('HN_posts_year_to_Sep_26_2016.csv','r', encoding='utf8')

# Note: Please replace [directory] below in the open() function with the link to your folder of choice.

```python
opened_file = open('[directory]hacker_news.csv','r')
hn = list(csv.reader(opened_file))
opened_file.close()
print(hn[:5]) # print the first 5 rows to test
```

[['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'],
 ['12224879',
  'Interactive Dynamic Video',
  'http://www.interactivedynamicvideo.com/',
  '386',
  '52',
  'ne0phyte',
  '8/4/2016 11:52'],
 ['10975351',
  'How to Use Open Source and Shut the Fuck Up at the Same Time',
  'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/',
  '39',
  '10',
  'josep2',
  '1/26/2016 19:30'],
 ['11964716',
  "Florida DJs May Face Felony for April Fools' Water Joke",
  'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/',
  '2',
  '1',
  'vezycash',
  '6/23/2016 22:20'],
 ['11919867',
  'Technology ventures: From Idea to Enterprise',
  'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429',
  '3',
  '1',
  'hswarna',
  '6/17/2016 0:01']]
  
  
