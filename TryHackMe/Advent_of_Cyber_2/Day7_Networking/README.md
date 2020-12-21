# [Day 7] Networking - The Grinch really did Steal Christmas

## Topics:

- IP protocol
- TCP/IP and UDP
    - 3-way handshake
- Wireshark
- PCAPS

### 1. Open "pcap1.pcap" in Wireshark. What is the IP address that initiates an ICMP/ping?
```
10.11.3.2
```
Once you have the pcap open, just type `ICMP` in the filter.

### 2. If we only wanted to see HTTP GET requests in our "pcap1.pcap" file, what filter would we use?
```
http.request.method == GET
```

### 3. Now apply this filter to "pcap1.pcap" in Wireshark, what is the name of the article that the IP address "10.10.67.199" visited?
```
reindeer-of-the-week
```
While searching through the packets, we can find some that get a `/posts/`. When inspecting the info of the packet we can find the name of the article.

### 4. Let's begin analysing "pcap2.pcap". Look at the captured FTP traffic; what password was leaked during the login process?
```
plaintext_password_fiasco
```
Apply a filter of `FTP`. We should now see the ftp interaction, with a certain packet giving us the password

### 5. Continuing with our analysis of "pcap2.pcap", what is the name of the protocol that is encrypted?
```
ssh
```

### 6. Analyse "pcap3.pcap" and recover Christmas! What is on Elf McSkidy's wishlist that will be used to replace Elf McEager?

When you analyze the file, you'll find a lot of packets, but we are only interested in the http ones because they are not encrypted. If we apply a filter to http we find a couple of packets. One of them contains a `christmas.zip` file, which we can download as an http object. Once downloaded and unzipped we find a some files, one of them being the wishlist with the answer.
