# [Day 22] Blue Teaming - Elf McEager becomes CyberElf

## Topics

- Password Manager (KeePass)
- CyberChef
- Encryption / Decryption

### 1. What is the password to the KeePass database?
```
thegrinchwashere
```
1. Open __CyberChed__ in a browser window
2. Drag the __Magic__ ingredient into the recipe
3. In the input section, type the weird folder name `dGhlZ3JpbmNod2FzaGVyZQ==`
4. On the output section you should see the password

### 2. What is the encoding method listed as the 'Matching ops'?
```
base64
```

### 3. What is the decoded password value of the Elf Server?

1. On KeePass, navigate to the Network section
2. Copy the password and paste it in CyberChef with the magic recipe
    - This time the decoder recognizes it as __Hex__

### 4. What is the decoded password value for ElfMail?

1. On KeePass, navigate to the eMail section
2. Repeat the same step as the last question
    - The encyrption this time was done using an __HTML Entity__

### 5. Decode the last encoded value. What is the flag?

1. On KeePass, navigate to the Recycle Bin
2. Double Click on the only entry available
3. Copy the Notes section. It shoul look like a bunch of numbers
4. Paste the text into the input sectoin of CyberChef
5. As the recipe, use two `From Charcode`, with both of them using the Comma as the delimiter and base 10
6. The decrypted message should be a github link. Enter the link to find the flag.