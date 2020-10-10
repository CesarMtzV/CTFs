# The Numbers
Cryptography

## Description
> The [numbers](https://jupiter.challenges.picoctf.org/static/f209a32253affb6f547a585649ba4fda/the_numbers.png)... what do they mean?

## Hints
> The flag is in the format PICOCTF{}

## Solution
The picture shows us the encrypted flag, you can tell that by the curly braces. 
With the help of the hint we know what the initial numbers translate to.

```
P -> 16
I -> 9
C -> 3
O -> 15
C -> 3
T -> 20
F -> 6
```
If you look closely, you'll notice that each number is assigned to its corresponding alphabet letter, like this:

```
A -> 1		N -> 14
B -> 2		O -> 15
C -> 3		P -> 16
D -> 4		Q -> 17
E -> 5		R -> 18
F -> 6		S -> 19
G -> 7		T -> 20
H -> 8		U -> 21
I -> 9		V -> 22
J -> 10		W -> 23
K -> 11		X -> 24
L -> 12		Y -> 25
M -> 13		Z -> 26
```
After translating each character, we find the flag.

Flag: PICOCTF{THENUMBERSMASON}