***Flag:*** <br>
flag
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
