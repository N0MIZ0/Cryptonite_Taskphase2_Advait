***Flag:*** <br>
picoCTF{7h3_p47h_70_5ucc355_e5fe3d4d} <br>
***Approaching the challenge:***
<br> The challenge description said how the flag is living in a particilar text file but the website is filtering out absolute paths. <br>
It was understanable that I had to use relative path of the text file to access the flag hence used 
```
././././flag.txt




```
![{63C0DA39-9C16-4D9C-842C-13E57C69754C}](https://github.com/user-attachments/assets/6dfeeb01-e92d-4825-b6b5-84ecba0e4e16) <br>
![{5CFBBF0C-DBC9-41FF-891D-11ED7C522320}](https://github.com/user-attachments/assets/081faf01-fe8b-479f-965c-1d8dad5e6c0a) <br>
But it didnt work, at the same time the first line of the website had **..** so instead I tried using 
```
../../../flag.txt


```
and it worked. <br>
![{FF4F92AA-A340-48F5-9063-353F4465F276}](https://github.com/user-attachments/assets/f84992e7-1d78-4d04-b21d-32a6dc2db989) <br>
![{B60E558B-3792-40C8-B714-049384C7BAD4}](https://github.com/user-attachments/assets/48f1e18f-b9d9-4091-a18f-924e61bec204)
