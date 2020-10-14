# Tapping
Cryptography

## Description
> Theres tapping coming in from the wires. What's it saying nc jupiter.challenges.picoctf.org 50538.

## Hints
1. What kind of encoding uses dashes and dots?
1. The flag is in the format PICOCTF{}

## Solution
When you connect to the server using the provided command, you receive the following input:
```console
.--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. ..--- -.... ---.. ...-- ---.. ..--- ....- -.... .---- ----- }
```
This type of encoding is called __Morse Code__, a language consisting of dots, slashes and spaces. This [online tool](https://morsecode.world/international/translator.html) translates it for you, you just have to delete the curly braces and add them later to the flag.

Flag: `PICOCTF{M0RS3C0D31SFUN2683824610}`