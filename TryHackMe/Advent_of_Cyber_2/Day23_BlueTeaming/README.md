# [Day 23] Blue Teaming - The Grinch strikes again!

## Topics 

- Ransomware
- Volume Shadow Copy Service
- Task Scheduler

### 1. Decrypt the fake 'bitcoin address' within the ransom note. What is the plain text value?
```
nomorebestfestivalcompany
```
1. On the desktop you should see a text file. Copy the fake bitcoin address and paste it on CyberChef.
2. As the recipe, use the magic ingredient

### 2. At times ransomware changes the file extensions of the encrypted files. What is the file extension for each of the encrypted files?
```
.grinch
```

### 3. What is the name of the suspicious scheduled task?
```
opidsfsdf
```
1. Open the Task Scheduler
2. There should be 5 tasks. One of them is the suspicious one

### 4. Inspect the properties of the scheduled task. What is the location of the executable that is run at login?
```
C:\users\administrator\desktop\opidsfsdf.exe
```

### 5. There is another scheduled task that is related to VSS. What is the ShadowCopyVolume ID?
```
7a9eea15-0000-0000-0000-010000000000
```
1. Click on the ShadowCopy Task
2. On the details below you should find the ID inside the braces in the Name section 

### 6. Assign the hidden partition a letter. What is the name of the hidden folder?
```
confidential
```
1. Open Disk Management
2. Right click the Backup volume and select `Change Drive Letter and Paths for Backup`
3. Click on Add and select a letter from the drop down menu. I chose `Z`. Once selected click ok.
4. Open File Explorer and search for the new Disk with the letter you just assigned. Go into that location.
5. You should see 2 folders, `database` and `vStockings`. Click on the View Tab on top and make sure to turn on `Hidden Items` to see the hidden folder called `confidential`

### 7. Right-click and inspect the properties for the hidden folder. Use the 'Previous Versions' tab to restore the encrypted file that is within this hidden folder to the previous version. What is the password within the file?

Follow the instructions and the answer should be pretty easy to find.