WEEK1&2 :
Q2: 
echo "enter the filename:"
read f
echo "enter the starting value:"
read s
echo "enter the ending value:"
read e
sed -n $s,$e\p $f

Q3:
echo "enter the word:"
ead word
for file in "$@"; do
if [-e "$file"]; then
grep -v "$word "$file > "$file.tmp" && mv "$file.tmp" "$file"
echo "Lines containing '$word' delted from $file"
else
echo " file not found"
fi
done

WEEK3:

Q1:
echo "files in curr directory:"
for i in *
do
echo $i
done

Q2:
for x in $*
do
if [ -f $x ]
then 
echo" $x is a file "
elif [-d $x ]
then echo " $x is a directory "
else
echo "entre valid filename or dir name"
fi done

WEEK4:

Q1:
for file in "$@"
 do
  if [ ! -f "$file" ]
  then
    echo "file doesn't exist"
    continue
  fi
  echo "word count for $file:"
  cat "$file" | tr '[:upper:]' '[:lower:]' | tr -cs '[:alpha:]' '\n' |sort | uniq -c| sort -nr
done 

Q2:
read-p "enter a number:"num 
fact=1
while [$num -gt 1]
do
fact=$((fact * num))
num=$((num-1))
done
echo $fact


Q3:
echo "enter directory name"
read dir
if [ -d $dir ]
then
echo "list of files in the directory"
ls –l $dir|egrep ‘^d’
else
echo "enter proper directory name"
fi
# WEEK5:
**1.Write a awk script to find the number of characters, words and lines in a file.
BEGIN{print "record.\t characters \t words"}**
#BODY section
{
len=length($0)
total_len =len
print(NR,":\t",len,":\t",NF,$0)
words =NF
}
END{
print("\n total")
print("characters :\t" total len)
print("lines :\t" NR)
}
Output:
Student@ubuntu:~$ awk –f cnt.awk ff1
Record words
1: 5: 1hello
Total
Characters:5
Lines:1

**2.Write a C Program that makes a copy of a file using standard I/O and system
calls**
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
void typefile (char *filename)
{
int fd, nread;
char buf[1024];
fd = open (filename, O_RDONLY);
if (fd == -1) {
perror (filename);
return;
}
while ((nread = read (fd, buf, sizeof (buf))) > 0)
write (1, buf, nread);
close (fd);
}
int
main (int argc, char **argv)
{
int argno;
for (argno = 1; argno < argc; argno )

typefile (argv[argno]);
exit (0);
}

Output:
student@ubuntu:~$gcc –o prg10.out prg10.c
student@ubuntu:~$cat > ff
hello
hai
student@ubuntu:~$./prg10.out ff
hello
hai

**3. Implement in C the following Unix commands using system calls A) cat B) mv
A)cat**
#include<sys/types.h>
#include<sys/stat.h>
#include<stdio.h>
#include<fcntl.h>
main( int argc,char *argv[3] )
{
int fd,i;
char buf[2];
fd=open(argv[1],O_RDONLY,0777);
if(fd==-argc)
{
printf("file open error");
}
else
{
while((i=read(fd,buf,1))>0)
{
printf("%c",buf[0]);
}
close(fd);
}
}
Output:
student@ubuntu:~$gcc –o prgcat.out prgcat.c

student@ubuntu:~$cat > ff
hello
hai
student@ubuntu:~$./prgcat.out ff
hello
hai

**b)MV**
Code:
#include<sys/types.h>
#include<sys/stat.h>
#include<stdio.h>
#include<fcntl.h>
main( int argc,char *argv[] )
{
int i,fd1,fd2;
char *file1,*file2,buf[2];
file1=argv[1];
file2=argv[2];
printf("file1=%s file2=%s",file1,file2);
fd1=open(file1,O_RDONLY,0777);
fd2=creat(file2,0777);
while(i=read(fd1,buf,1)>0)
write(fd2,buf,1);
remove(file1);
close(fd1);
close(fd2);
}
Output:
student@ubuntu:~$gcc –o mvp.out mvp.c
student@ubuntu:~$cat > ff
hello
hai
student@ubuntu:~$./mvp.out ff ff1
student@ubuntu:~$cat ff
cat:ff:No such file or directory
student@ubuntu:~$cat ff1
hello
hai
