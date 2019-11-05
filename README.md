# hacker-news-posts  

Project using CSV Reader, list slicing, and various functions to analyze a CSV dataset in Python

# Project Details:  
Hacker News is a site where users can submit or vote on posts about technology and computer science, similiar to Reddit. Hacker News is extremely popular in technology and startup circles, and posts that make it to the 'front page' of Hacker News generate tens of thousands, if not hundreds of thousands of views.

There are two types of posts we will be focusing on in this project, Ask HN and Show HN posts.
Ask HN: The user submits a question in which other users can provided a response or vote on their favorite response.
Show HN: The user does not have a question but wants to show something ineresting, such as a project or an article

We'll specifically compare these two types of posts to determine the following:  
1. Do Ask HN or Show HN posts receive more comments on average?  
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
{'09': 251,  
 '13': 1253,  
 '10': 793,  
 '14': 1416,  
 '16': 1814,  
 '23': 543,  
 '12': 687,  
 '17': 1146,  
 '15': 4477,  
 '21': 1745,  
 '20': 1722,  
 '02': 1381,  
 '18': 1439,  
 '03': 421,  
 '05': 464,  
 '19': 1188,  
 '01': 683,  
 '22': 479,  
 '08': 492,  
 '04': 337,  
 '00': 447,  
 '06': 397,  
 '07': 267,  
 '11': 641}  
 
# Calculating the Average Number of Comments for Ask HN Posts by Hour
```python
avg_by_hour = []

# for each hr, we're going to create an individual list holding the hr and the average
# (by taking the total amt of comments for that # hour and dividing it by the number of posts in that hr)

for hr in comments_by_hour:
    avg_by_hour.append([hr, comments_by_hour[hr] / counts_by_hour[hr]])

print(avg_by_hour)
```
[['09', 5.5777777777777775],  
 ['13', 14.741176470588234],  
 ['10', 13.440677966101696],  
 ['14', 13.233644859813085],  
 ['16', 16.796296296296298],  
 ['23', 7.985294117647059],  
 ['12', 9.41095890410959],  
 ['17', 11.46],  
 ['15', 38.5948275862069],  
 ['21', 16.009174311926607],  
 ['20', 21.525],  
 ['02', 23.810344827586206],  
 ['18', 13.20183486238532],  
 ['03', 7.796296296296297],  
 ['05', 10.08695652173913],  
 ['19', 10.8],  
 ['01', 11.383333333333333],  
 ['22', 6.746478873239437],  
 ['08', 10.25],  
 ['04', 7.170212765957447],  
 ['00', 8.127272727272727],  
 ['06', 9.022727272727273],  
 ['07', 7.852941176470588],  
 ['11', 11.051724137931034]]  
 
 # Sorting and Printing Values from a List of Lists
 To analyze the above data a little easier, let's sort it by the average per hour in descending order:
 ```python
swap_avg_by_hour = []

# We're first going to swap the columns on a new list so that the average is first (Ex. [5.5777777777777775, '09'])
for row in avg_by_hour:
    swap_avg_by_hour.append([row[1], row[0]])

# We can then use the built-in 'sorted' function which sorts by the first column, now being the average.
# The default sort parameter is by ascending order. We can add the optional argument 'reverse=True' to sort by descending order.
sorted_swap = sorted(swap_avg_by_hour, reverse=True)

print(sorted_swap)
```

[[38.5948275862069, '15'],  
 [23.810344827586206, '02'],  
 [21.525, '20'],  
 [16.796296296296298, '16'],  
 [16.009174311926607, '21'],  
 [14.741176470588234, '13'],  
 [13.440677966101696, '10'],  
 [13.233644859813085, '14'],  
 [13.20183486238532, '18'],  
 [11.46, '17'],  
 [11.383333333333333, '01'],  
 [11.051724137931034, '11'],  
 [10.8, '19'],  
 [10.25, '08'],  
 [10.08695652173913, '05'],  
 [9.41095890410959, '12'],  
 [9.022727272727273, '06'],  
 [8.127272727272727, '00'],  
 [7.985294117647059, '23'],  
 [7.852941176470588, '07'],  
 [7.796296296296297, '03'],  
 [7.170212765957447, '04'],  
 [6.746478873239437, '22'],  
 [5.5777777777777775, '09']]  

In conclusion, we find that hour 15:00 (3:00 PM) has the highest average of comments at 38.59 per post.
We also notice that the afternoon-nighttime has generally more comments per post with a weird exception at 10:00 (10:00 AM). Remember, this dataset is limited and was cleaned of any posts which did not have any comments. It may or may not have had an affect on these averages.
