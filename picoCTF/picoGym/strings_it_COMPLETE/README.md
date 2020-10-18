# Strings it
General Skills

## Description
> Can you find the [flag](https://jupiter.challenges.picoctf.org/static/94d00153b0057d37da225ee79a846c62/strings) in file without running it?

## Hints
1. [strings](https://linux.die.net/man/1/strings)

## Solution
We use the `strings` function and then use grep to find the flag in an easier manner:

```bash
#!/bin/bash

strings strings | grep "picoCTF" > flag.txt
```

Flag: `picoCTF{5tRIng5_1T_d66c7bb7}`