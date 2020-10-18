# Glory of the Garden
Forensics

## Description
> This [garden](https://jupiter.challenges.picoctf.org/static/d0e1ffb10fc0017c6a82c57900f3ffe3/garden.jpg) contains more than it seems

## Hints
1. What is a hex editor?

## Solution
Upon reading the first hint, we can assume we'll need a hex editor to find the flag.
A __hex editor__ is a program that allows us to manipulate the binary data of a file, including a picture. You can use [this one](https://hexed.it/) for solving this challenge.
If you open the picture like you normally would, you'll find nothing out of the ordinary, it's just a picture. BUT, if you open it on the hex editor you'll discover all the hex values that make that image.
The flag is found at the bottom.

Flag: `picoCTF{more_than_m33ts_the_3y3eBdBd2cc}`