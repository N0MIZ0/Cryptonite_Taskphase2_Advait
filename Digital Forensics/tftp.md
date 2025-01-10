***Flag:***<br>
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
<br>
***Approaching the Challenge:*** <br>
After going through the material, it was clear that the provided file was a **pcapng**. <br>
PCAP is basically picture capture analysis, but not like steganography. <br>
PCAP is where network traffic can be stored. I proceeded downloading **wireshark** hoping I'll get the flag, but it had lots of data stored among which finding the flag was manually not practical. Tried viewing them in statistical view.
I then just went to different viewing options and checking the "netowork traffic" hoping to eye the flag.
;-; <br>
![{DD6D6C6A-EA48-481D-BCC0-9F83B0CC920D}](https://github.com/user-attachments/assets/526e804e-7e3f-4987-a0e1-e714fc06cfe5)
<br>
After this I had to export the objects out of the file to get me to some lead and found these:
<br>
![{C14A59F2-C48B-45AD-B8ED-6475B9EC337A}](https://github.com/user-attachments/assets/4052fb31-353c-4cc1-87e4-2b7b910e7e47)
<br>
I found the this on the **intructions text file**:
``` GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA```
I used cryptii and tried decoding it as ceaser, but it was ROT13. The decoded message was:
``` TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAGTRANSFER.FIGURE OUT A WAYTO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN ```
It was a piece of information but I could not make much outta it, so I went to the images to check if I could do something with them.
Tried to find hidden strings using Stegonline.
Opened the flag **plan text file** which was ROT13 again, and it translated to:
``` IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS ```
So we can conclude that the flag is hidden in one of the 3 photos extracted. <br>
Since the online service of StegOnline was not working I had to install steghide on wsl to go through the images thouroughly.
<br>
Used a guide for the following and finally found the flag.
```
advait@Advait:/mnt/c/Users/advai/Documents$ steghide extract -sf picture1.bmp -p DUEDILIGENCE
steghide: could not extract any data with that passphrase!
advait@Advait:/mnt/c/Users/advai/Documents$ steghide extract -sf picture2.bmp -p DUEDILIGENCE

^C
advait@Advait:/mnt/c/Users/advai/Documents$ steghide extract -sf picture2.bmp -p DUEDILIGENCE
steghide: could not extract any data with that passphrase!
advait@Advait:/mnt/c/Users/advai/Documents$ steghide extract -sf picture3.bmp -p DUEDILIGENCE
wrote extracted data to "flag.txt".
advait@Advait:/mnt/c/Users/advai/Documents$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```
Resource: https://www.youtube.com/watch?v=VmSgalNMw_Y
