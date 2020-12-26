# [Day 18] Reverse Engineering - The bits of Christmas

## Topics

- .NET Framework
- ILSpy

### 1. Open the "TBFC_APP" application in ILspy and begin decompiling the code

Follow the guide in order to connect to the windows machine. Once inside, open ILspy and use CTRL+O to open the target app.

### 2. What is Santa's password?
```
santapassword321
```
1. On the left side you should see all the program files. There's one called __Crack Me__. Open it
2. There are two components. The __AboutForm__ and the __MainForm__
3. Inside the Main Form, search for a function called `buttonActivate_Click`
4. In that function you should find the flag for the next question, as well as a reference to the password `ref <Module>.??_C@_0BB@IKKDFEPG@santapassword321@` in the second line.
5. Click on the password, it should take you to another function in another module
6. The only line in the new file contains a comment in HEX. Decode that string to find the correct password

### 3. Now that you've retrieved this password, try to login...What is the flag?

Find it in the source code of the function or after entering the password into the app.