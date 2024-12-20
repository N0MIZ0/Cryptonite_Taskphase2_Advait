***Flag:***
<br>
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_63191ce6}
<br>
***Approaching the challenge:*** <br>
Connected the shell to ``` c mimas.picoctf.net 64891 ``` and tried lurking around different types of burgers. <br>
The output was same for all of them except for one, *Gr%114d_Cheese*
The source was a C file, so I opened it and check for what could be done.  <br>
From the source code it was clear that our objective was to feed patrick i.e, get the flag by making sure patrick is not hungry. That is only possible
if certain conditions are met. 
<br>
```
if (count > 2 * BUFSIZE) {
            serve_bob();
```
Hence the burger we input should be greater than 2 times BUFSIZE(>64) <br>
So I input a random string of many characters, but it was not of any good. <br>
Apprently the ```%114``` in grilled cheeze option is a format specfier which can cause either unexpected or very large output (hence makes the string larger than 64) <br>
Now it made sense why my previous input of *Gr%114d_Cheese* gave me a different output. <br>
Further I had nwo unlocked 3 more types of burgers. <br>
The later code just informed that it would post error if the input buregr we gave was not part of the list. Since the list was small, I checked in with all the buregrs 
and got the flag at the last one.

*I could not open the binary file, but I believe the answer to the final list of burgers was present in that file*



```
root@Advait:~# nc mimas.picoctf.net 64891
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: Gr%114d_Cheese
Gr                                                                                                           4202954_Cheese
Good job! Patrick is happy! Now can you serve the second customer?
Sponge Bob wants something outrageous that would break the shop (better be served quick before the shop owner kicks you out!)
Please choose from the following burgers: Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
Enter your recommendation: Cla%sic_Che%s%steak
ClaCla%sic_Che%s%steakic_Che(null)
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_63191ce6}
```
