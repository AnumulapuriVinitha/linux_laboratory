# linux_laborator
# WEEK-6
**1.Write a C program to emulate the Unix ls-l command.** 
  
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>
int main()
{
int pid;           
pid = fork();     
if(pid < 0 )
{                                 
printf("\n Fork failed \n ");
exit(-1);
}
else if(pid == 0 )
{                     
execlp("/bin/ls", "ls", "-l", NULL );  
}
else
{                             
wait(NULL);              
printf("\n child complete \n ");
exit(0);
}
}

**2.C program to list for every file in a directory & its inode number and file name**

#include<stdlib.h>
#include<stdio.h>
#include<string.h>
int main(int argc, char *argv[])
{
char d[50];
if(argc==2)
{
bzero(d,sizeof(d));
strcat(d,"ls ");
strcat(d,"-i ");
strcat(d,argv[1]);
system(d);
}
else
printf("\nInvalid No. of inputs");
 }

 **3.Write a C Program that demonstrates redirection of standard output to a file .EX: ls > f1.**

#include<stdlib.h>
#include<stdio.h>
#include<string.h>
 int main(int argc, char *argv[])
{
char d[50];
if(argc==2)
{
bzero(d,sizeof(d));
strcat(d,"ls ");
strcat(d,"> ");
strcat(d,argv[1]);
system(d);
}
else
printf("\nInvalid No. of inputs");
}
