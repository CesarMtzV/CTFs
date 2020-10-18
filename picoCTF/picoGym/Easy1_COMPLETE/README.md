# Easy1
Cryptography

## Description
> The one time pad can be cryptographically secure, but not when you know the key. Can you solve this? We've given you the encrypted flag, key, and a table to help UFJKXQZQUNB with the key of SOLVECRYPTO. Can you use this [table](https://jupiter.challenges.picoctf.org/static/1fd21547c154c678d2dab145c29f1d79/table.txt) to solve it?.

## Solution
This type of cipher is known as [Vigenère Cipher](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher), because we know the key `SOLVECRYPTO` we can easily decrypt this using an [online tool](https://www.boxentriq.com/code-breaking/vigenere-cipher).

Flag: `picoCTF{CRYPTOISFUN}`