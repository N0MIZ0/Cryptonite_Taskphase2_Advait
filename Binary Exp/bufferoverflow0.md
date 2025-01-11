***Flag:*** <br>
picoCTF{ov3rfl0ws_ar3nt_that_bad_ef01832d} <br>
***Approaching the challenge*** <br>
Connected the webshell to the host, and any input I give would exit the code. <br>
Source code contained a C file:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
  printf("%s\n", flag);
  fflush(stdout);
  exit(1);
}

void vuln(char *input){
  char buf2[16];
  strcpy(buf2, input);
}

int main(int argc, char **argv){
  
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("%s %s", "Please create 'flag.txt' in this directory with your",
                    "own debugging flag.\n");
    exit(0);
  }
  
  fgets(flag,FLAGSIZE_MAX,f);
  signal(SIGSEGV, sigsegv_handler); // Set up signal handler
  
  gid_t gid = getegid();
  setresgid(gid, gid, gid);


  printf("Input: ");
  fflush(stdout);
  char buf1[100];
  gets(buf1); 
  vuln(buf1);
  printf("The program will exit now\n");
  return 0;
}
```
So basically to inout, they have used **gets** which can allow a larger input than the size declared to **buf1** which is 100. Hence by doing so we initiate a
segmentation error resulting in the flag. <br>
Segmentation errors are specific types of errors where the program tries to access/locate memory in an invalid way.
<br> ***Proofs:*** <br>
![{FD0B109A-51E1-4DE4-8A17-7AA2625C926B}](https://github.com/user-attachments/assets/19e09f5b-67c9-4d42-a0fd-222e3700c833)
