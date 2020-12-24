# [Day 14] Special by TheCyberMentor - Where's Rudolph?

## Topics

 - OSINT

### 1. What URL will take me directly to Rudolph's Reddit comment history?
```
https://www.reddit.com/user/IGuidetheClaus2020/comments/
```
We can use any of the provided tools to search for the username `IGuidetheClaus2020`. In my case I used [namechk](namechk.com). From there we know that Rudolph has a reddit account, and when visited we should get the URL we need.

### 2. According to Rudolph, where was he born?
```
Chicago
```
One of the reddit comments tells the answer.

### 3. Rudolph mentions Robert.  Can you use Google to tell me Robert's last name?
```
may
```
Rudolph tells us that his creator is called Robert. If we google `rudolph the red nosed reindeer creator robert` we should get the answer right away.

### 4. On what other social media platform might Rudolph have an account?
```
twitter
```
In one of the comments he mentions how sometimes twitter can be so ..lol, so we can assume he uses twitter.

### 5. What is Rudolph's username on that platform?
```
IGuideClaus2020
```
Search for `IGuideTheClaus2020` on twitter and filter by people. You should find the account with a picture of rudolph.

### 6. What appears to be Rudolph's favorite TV show right now?
```
bachelorette
```
Just by looking at the twitter feed you can identify his favorite show.

### 7. Based on Rudolph's post history, he took part in a parade.  Where did the parade take place?
```
chicago
```
One of the posts is a higher resolution picture of him in the parade. When you click the link it takes you to the picture. We're going to look in the metadata of the image with an online [tool](http://exif.regex.info/). You can either use the url of the photo or you can download it to upload it to the program.\
When the program finishes reading the data you should get some coordenates, putting them on Google takes you to Chicago.

### 8. Okay, you found the city, but where specifically was one of the photos taken?
```
41.891815, -87.624277
```

### 9. Did you find a flag too?

The flag should be in the copyright section of the image

### 10. Has Rudolph been pwned? What password of his appeared in a breach?
```
spygame
```
Unfortunately at the time of this writeup, [schylla.sh](https://scylla.sh/) is down. This is the site where we could find this answer. You can watch TheCyberMentor's walkthrough to see scylla in action. All we had to do was to search for his email (which is found in his twitter bio) like this `email:rudolphthered@hotmail.com`

### 11. Based on all the information gathered.  It's likely that Rudolph is in the Windy City and is staying in a hotel on Magnificent Mile.  What are the street numbers of the hotel address?
```
540
```
We know the coordenates of the picture, and based on his twitter feed we know he stayed at a Marriott Hotel. Search for the nearest Marriott Hotel to the coordenates we have and the hotel number should be found in the description.