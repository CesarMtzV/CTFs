# [Day 20] Blue Teaming - PowershELlf to the rescue

## Topics

- Powershell

### 1. Search for the first hidden elf file within the Documents folder. Read the contents of this file. What does Elf 1 want?
```
2 front teeth
```
1. In the Documents directory, run `Get-ChildItem -File -Hidden` to list all items in the folder.
2. Open the target file with `Get-Content .\e1fone.txt`

### 2. Search on the desktop for a hidden folder that contains the file for Elf 2. Read the contents of this file. What is the name of that movie that Elf 2 wants?
```
Scrooged
```
1. Change to the desktop with `Set-Location -Path 'C:\Users\mceager\Desktop\'`
2. List all the hidden folders in the desktop `Get-ChildItem -Hidden -Directory`
3. List the files in the hidden folder `Get-ChildItem '.\elf2wo\'`
4. Print the contents of the file `Get-Content '.\elf2wo\e70smsW10Y4k.txt'`

### 3. Search the Windows directory for a hidden folder that contains files for Elf 3. What is the name of the hidden folder? (This command will take a while)
```
3lfthr3e
```
1. Change into the Windows system directory `Set-Location -Path 'C:\Windows\System32\'`
2. Search for the hidden folder using the filter provided to us in the hint `ls -Hidden -Directory -Filter '*3*'`

### 4. How many words does the first file contain?
```
9999
```
1. Change into the new found folder `cd .\3lfthr3e`
2. Print the file and pipe it into the Measure-Object function `cat .\1.txt | Measure-Object -Word`

### 5. What 2 words are at index 551 and 6991 in the first file?
```
Red Ryder
```
1. Find the firt word with `(cat .\1.txt)[551]`
2. Find the second word with `(cat .\1.txt)[6991]`

### 6. This is only half the answer. Search in the 2nd file for the phrase from the previous question to get the full answer. What does Elf 3 want? (use spaces when submitting the answer)

1. Use `Select-String .\2.txt -Pattern "redryder"` to find that string in the file
