# Lets Warm Up
General Skills, 50 points

## Description
> If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?

## Solution
We could use an ASCII table that has Hex values in order to find the corresponding character, or we could use Python.
In this case we'll use the Python funnction `chr()` to find the flag.

```python
>>> chr(0x70)
'p'
```

Flag: `picoCTF{p}`