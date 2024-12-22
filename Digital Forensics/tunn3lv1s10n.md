***Flag:*** <br>
picoCTF{qu1t3_a_v13w_2020}
<br>
***Approaching the challenge:*** <br>
Tried ``` strings tunn3l_v1s10n | grep picoCTF ``` but did not work. <br>
I was cluless at this point so I browsed the internet regarding digital forensics flags where they mentioned saying that files are often incorrect headings meaning
the format,size or the dimension is incorrect/corrupted. They also call this ``` Malformed Header ```. <br> Since grep did not work, we can assume that the file has a malformed header. <br>
So to understand about that we generally use the ``` exiftool file_name ``` command which gives us the entire details about the file. <br>
So here the file type was **bmp***. <br>
Now, we have a bmp image file with a malformed header. <br>
Looking at the hexadecimal source of the file also confirmed us that this is a bmp image since it started of with **42 4D** which means it's a bmp image. <br>
![{E214D3C9-A71F-4C42-8064-4F1C8F05F791}](https://github.com/user-attachments/assets/f18064c0-7cbb-4977-a120-99b0faf0d281)
<br>
There's a part of header called DIB header which means Device Independent Bitmap header storing image height width etc. <br>
The most common DIB header format is BITMAPINFOHEADER, which is 40 bytes long. However the DIB header had ```BA D0``` which is ``` xd0ba```. 
This transaltes to 53434 bytes, much larger than 40. So we convert **BA D0** to just 40 bytes by replacing them with **28 00**. <br>
After doing this, we get an updated image but it did not havee the flag. <br>
![{0C30E78F-DBB8-4886-A313-E80D8D67A109}](https://github.com/user-attachments/assets/2f5dc133-0aef-4c91-91cd-173b4fb9ec00)
<br>
But this seemed like an incomplete image and we needed to set the dimensions properly. Since this is a bmp image, the data we already know is that
54 bytes header and 3 bytes per pixer (standard for bmp files) and additonally the file is 2884750 bytes and 1134*306 pixels. <br>
All we need to do is increase the hegiht of the image so we shall increase the height from 306pixels to something more. Knowing all this we can simply calculate height:
``` (2884750 - 54) // (3*1134) = 848 ```
848 in hexadecimal is ```0x350```, so input 0x350 in the header and got the flag....

RESOURCE USED: https://github.com/apoirrier/CTFs-writeups/blob/master/PicoCTF/Forensics/tunn3l_v1s10n.md
