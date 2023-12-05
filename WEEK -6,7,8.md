# WEEK-6
# 1.Write a C program to emulate the Unix ls-l command.
  
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

# 2.C program to list for every file in a directory & its inode number and file name

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

 # 3.Write a C Program that demonstrates redirection of standard output to a file .EX: ls > f1.

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

# WEEK7
### 1.Write a C program to create a child process and allow the parent to display “parent” and the
child to display “child” on the screen.
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

int main(void) {
    pid_t pid;
    int status;

  printf("Hello World!\n");

  pid = fork();

  if (pid == -1) { /* check for error in fork */
        perror("bad fork");
        exit(1);
    }

  if (pid == 0) {
        printf("I am the child process.\n");
        // Add child process code here
        exit(0); // Make sure the child process exits after its work is done
    } else {
        waitpid(pid, &status, 0); /* parent waits for child to finish */
        printf("I am the parent process.\n");
    }

   return 0;
}

### 2.Write a C program to create a Zombie process.
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h> // Added for printf

int main() {
    pid_t child_pid;
    child_pid = fork();

  if (child_pid > 0) {
        // This is the parent process
        printf("Parent process. PID: %d\n", getpid());
        sleep(60);
    } else if (child_pid == 0) {
        // This is the child process
        printf("Child process. PID: %d\n", getpid());
        exit(0);
    } else {
        // Error handling if fork fails
        perror("Fork failed");
        return 1;
    }

  return 0;
}

# WEEK-8 

### 1.write a c program that illustrates how an orphan is created
#include <stdio.h>
#include <unistd.h> // Include this header for the sleep function
int main() // Use int main() instead of main()
{

  int pid;
    printf("I'm the original process with PID %d and PPID %d.\n", getpid(), getppid());
    pid = fork();
    if (pid != 0)
    {
        printf("I'm the parent with PID %d and PPID %d.\n", getpid(), getppid());
        printf("My child's PID is %d\n", pid);
    }
    else
    {
        sleep(4);
        printf("I'm the child with PID %d and PPID %d.\n", getpid(), getppid());
    }
    printf("PID %d terminates.\n", getpid());
    return 0; // Add a return statement at the end of main()
}
### O/P: 
I am the original process with PID2242 and PPID1677.
I am the parent with PID2242 and PPID1677
My child’s PID is 2243
PID2243 terminates.
$  I am the child with PID2243 and PPID1.
PID2243 termanates.

### 2.Write  a program  that illustrates how to execute two commands concurrently with a command pipe.
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <stdlib.h>

int main()
{

  int pfds[2];
    char buf[80];  // Corrected the size of the buffer
    if (pipe(pfds) == -1)
    {
        perror("pipe failed");
        exit(1);
    }
    if (!fork())
    {
        close(1);
        dup(pfds[1]);  // Corrected the syntax error
        close(pfds[0]);  // Close unused read end in the child process
        system("ls -l");
    }
    else
    {
        close(pfds[1]);  // Close unused write end in the parent process
        printf("parent reading from pipe \n");
        while (read(pfds[0], buf, sizeof(buf)) > 0)  // Corrected the size argument in read
            printf("%s \n", buf);
    }
    return 0;  // Added a return statement at the end of the main function
}
### O/P: 
Parent reading from pipe
Total 24
-rwxrwxr-x l student student 5563Aug 3 10:39 a.out
-rw-rw-r—l
Student student 340 jul 27 10:45 pipe2.c
-rw-rw-r—l student student
Pipes2.c
-rw-rw-r—l student student 401 34127 10:27 pipe2.c
student

### 3.Write a C programs that illustrate communication between two unrelated processes using named pipe
#include<stdio.h>
#include<stdlib.h>
#include<errno.h>
#include<unistd.h>
int main()
{

  int pfds[2];
    char buf[30];
    if(pipe(pfds)==-1)
    {
        perror("pipe");
        exit(1);
    }
     printf("writing to file descriptor #%d\n", pfds[1]);
    write(pfds[1],"test",5);
    printf("reading from file descriptor #%d\n ", pfds[0]);
    read(pfds[0],buf,5);
    printf("read\"%s\"\n" ,buf);
}
### O/P: 
writing to file descriptor #4
reading from file descriptor #3
read"test"
