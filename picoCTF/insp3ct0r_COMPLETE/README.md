# Insp3ct0r
Web Exploitation

## Description
> Kishor Balan tipped us off that the following code may need inspection: https://jupiter.challenges.picoctf.org/problem/38429/

## Hints
1. How do you inspect web code on a browser?
1. There's 3 parts

## Solution
Thanks to the hints, we know we have to use the inspector on the given website.
To open the inspector just press right click on the mouse and select the Inspect option.
If you explore the complete HTML file, you'll find the first part of the flag.
```html
<!--Html is neat. Anyways have 1/3 of the flag: picoCTF{tru3_d3-->
```

At the start of the HTML you may have noticed that they are referencing a `mycss.css` and `myjs.js`.
Those files are the ones that contain the other two parts. Depending on the browser you are using, the inspector may differ, but in the end you have to find those 2 extra files and find the other 2 flag parts to complete the challenge.

Flag: `picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?f10be399}`
