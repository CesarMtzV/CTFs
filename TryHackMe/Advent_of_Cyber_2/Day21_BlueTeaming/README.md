# [Day 21] Blue Teaming - Time for some ELForensics

## Topics

- File hashes
- Alternate Data Streams

### 1. Read the contents of the text file within the Documents folder. What is the file hash for db.exe?
```
596690FFC54AB6101932856E6A78E3A1
```
1. Go into the Documents folder in powershell `cd .\Documents\`
2. Do `ls` to find the target file
3. Read the text file with `cat '.\db file hash.txt'`

### 2. What is the file hash of the mysterious executable within the Documents folder?
```
5F037501FB542AD2D9B06EB12AED09F0
```
Get the file hash for the `deebee.exe` file with `Get-FileHash -Algorithm MD5 .\deebee.exe`

### 3. Using Strings find the hidden flag within the executable?

1. Run `C:\Tools\strings64.exe -accepteula .\deebee.exe`
2. Somewhere in the output should be the flag

### 4. What is the flag that is displayed when you run the database connector file?

1. Find the stream name with `Get-Item -Path .\deebee.exe -Stream *`. It should be near the bottom of the output
2. Run the database connector with `wmic process call create $(Resolve-Path .\deebee.exe:hidedb)`
3. A command prompt should pop up. After a few seconds you should get the flag.