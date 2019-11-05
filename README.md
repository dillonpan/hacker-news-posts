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
First we open the file and assign it to a variable(opened_file). Afterwards, we can use the Reader function within the CSV package to appoint the file as a CSV. We can then assign the CSV as a list of rows and seperate the first row (the column headers) from the rest of the data. Lastly, we can just close the the open() link.  

Note: If you run into an error named UnicodeDecodeError, add encoding="utf8" to the open() function.
Example: open('HN_posts_year_to_Sep_26_2016.csv','r', encoding='utf8')

# Note: Please replace [directory] below in the open() function with the link to your folder of choice.

```python
opened_file = open('[directory]hacker_news.csv','r')
hn = list(csv.reader(opened_file))
headers = hn[0]
hn = hn[1:]
opened_file.close()
print(headers) # print the header to test =
print(hn[:3]) # print the first 3 rows to test
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
  '6/23/2016 22:20']]  
  
# Extracting Ask HN and Show HN Posts
We can create three seperate lists to filter between the Ask HN, Show HN, and other posts apart from each other. The title of both Ask HN and Show HN posts will generally start as accordingly, (EX. "Ask HN: I was wondering...etc..etc")
```python
ask_posts = []
show_posts =[]
other_posts = []

for post in hn:
    title = post[1] # the Title of the post is listed on the 2nd column of the CSV
    if title.lower().startswith("ask hn"):
        ask_posts.append(post)
    elif title.lower().startswith("show hn"):
        show_posts.append(post)
    else:
        other_posts.append(post)
        
print(len(ask_posts))
print(len(show_posts))
print(len(other_posts))
```
1744
1162
17194


# Calculating the Average Number of Comments for Ask HN and Show HN Posts
Average number of comments for ASK HN posts:
```python
total_ask_comments = 0
for post in ask_posts:
    total_ask_comments += int(post[4]) # comments are on column 5 of the CSV. Add up the number of comments per post for the total
avg_ask_comments = total_ask_comments / len(ask_posts) # total amount of comments divided by the number of ASK HN posts
print(avg_ask_comments)
```
14.038417431192661

Average number of comments for SHOW HN posts:
```python
total_show_comments = 0
for post in show_posts:
    total_show_comments += int(post[4])
avg_show_comments = total_show_comments / len(show_posts)
print(avg_show_comments)
```
10.31669535283993

After finding the average number of comments per post for both types of posts, we can deteremine that that ASK HN receives more comments usually. Makes sense since ASK HN posts are specifically requesting reponses/comments from other users.

# Finding the Amount of Ask Posts and Comments by Hour Created
Next, we'll determine if we can maximize the amount of comments an ask post receives by creating it at a certain time. First, we'll find the amount of ask posts created during each hour of day, along with the number of comments those posts received. Then, we'll calculate the average amount of comments ask posts created at each hour of the day receive.

```python
import datetime as dt

result_list = []

# We're putting each post in to a seperate list which holds the date+time a post was created and that post's number of comments
# Afterwards, we put each individual list in to the result_list. Essentially, it's a bunch of seperate lists within one large list
for post in ask_posts:
    result_list.append(
        [post[6], int(post[4])]  # each individual list contains, in order: date+time creation, number of comments
    )

comments_by_hour = {} # dictionary to keep track of how many comments in each hour
counts_by_hour = {} # dictionary to keep track of how many posts in each hour
date_format = "%m/%d/%Y %H:%M" # Regex formatting that works with the datetime package

for each_row in result_list: # taking each list within the large result_list, one at a time
    date = each_row[0] # taking the creation date+time 
    comment = each_row[1] # taking the number of comments
    time = dt.datetime.strptime(date, date_format).strftime("%H") # filtering out the Hour
    if time in counts_by_hour:
        comments_by_hour[time] += comment
        counts_by_hour[time] += 1
    else:
        comments_by_hour[time] = comment
        counts_by_hour[time] = 1

print(comments_by_hour)
```

