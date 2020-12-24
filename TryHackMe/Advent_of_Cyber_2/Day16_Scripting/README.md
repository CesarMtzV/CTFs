# [Day 16] Scripting - Help! Where is Santa?

## Topics

- Putting scripting skills to the test

### 1. What is the port number for the web server?
```
8080
```
For this we do a simple nmap scan `nmap -sV 10.10.x.x`

__important nmap output:__
```
Not shown: 999 closed ports
PORT     STATE SERVICE  VERSION
8000/tcp open  http-alt uvicorn
```

### 2. Without using enumerations tools such as Dirbuster, what is the directory for the API?  (without the API key)
```
api
```
You can just guess this one, but a more secure approach would be to scan the website and search for the hidden link that is mentioned. This can be done manually or with a python script like in day 15.

__python script to scrape the links:__
```python
from bs4 import BeautifulSoup, SoupStrainer
import requests

url = "http://10.10.x.x:8000/static/index.html"

page = requests.get(url)    
data = page.text
soup = BeautifulSoup(data)

for link in soup.find_all('a'):
    print(link.get('href'))
```
Run the script with `python3 scriptName.py`. If you don't have the dependencies, install them with `pip3 install xxx`.\
The link that we want should pop up: `http://machine_ip/api/api_key`

### 3. Where is Santa right now?

For this question and the next one, we need to find the api key, but too many guesses will end up with out IP banned from the site. We can use a python script to brute force only the odd numbers instead of all 100 of them.

__python brute forcing script:__
```python
import requests

for api_key in range(1,100,2):
	print(f'API KEY: {api_key}')
	html = requests.get(f'http://10.10.241.232:8000/api/{api_key}')
	print(html.text)
```
This code uses the `requests` library to get the website. We are just looping from 1 to 100 with a jump of 2, so we would start with 1, then 3, then 5 and so on. Then we replace that number in the url string and we print the html response. Eventually we should find the api key because it'll have a different response than the rest.


### 4. Find out the correct API key. Remember, this is an odd number between 0-100. After too many attempts, Santa's Sled will block you. 
```
57
```