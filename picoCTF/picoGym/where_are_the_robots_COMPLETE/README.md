# Where are the robots
Web Exploitation

## Description
> Can you find the robots? https://jupiter.challenges.picoctf.org/problem/13758/ (link) or http://jupiter.challenges.picoctf.org:13758

## Hints
1. What part of the website could tell you where the creator doesn't want you to look?

## Solution
This challenge requires us to use the [robots.txt](https://support.google.com/webmasters/answer/6062608?hl=en) file inisde that website.
When you open the site, you just have to add `robots.txt` to the URL on top, like this:
```
https://jupiter.challenges.picoctf.org/problem/13758/robots.txt
```
When you hit enter, you'll find this:
```
User-agent: *
Disallow: /8028f.html
```
We'll take the `/8028f.html` and replace the `robots.txt` with it, like this:
```
https://jupiter.challenges.picoctf.org/problem/13758/8028f.html
```
You'll find the flag in the page that loads.

Flag: `picoCTF{ca1cu1at1ng_Mach1n3s_8028f}`