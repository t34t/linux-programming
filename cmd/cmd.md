## c-programming for execute shell command


### c程序执行shell命令
一般来说，在linux系统中使用c程序调用shell命令有以下三种常见的方法system、popen、exec
system不需要用户再创建进程，因为它已经封装好了，直接加入shell命令即可
popen执行shell命令，其开销比system小
exec需要用户fork/vfork进程，然后exec所需的shell命令

#### system
system() 会调用 fork() 产生子进程，由子进程来调用 /bin/sh -c string 来执行参数 string 字符串所代表的命令，此命令执行完后随即返回原调用的进程。在调用 system() 期间 SIGCHLD 信号会被暂时搁置，SIGINT 和 SIGQUIT 信号则会被忽略

如果 system()在 调用 /bin/sh 时失败则返回 127，其他失败原因返回 -1。若参数 string 为空指针（NULL），则返回非零值。如果 system() 调用成功则最后会返回执行 shell 命令后的返回值，但是此返回值也有可能为 system() 调用 /bin/sh 失败所返回的 127，因此最好能再检查 errno 来确认执行成功。

在编写具有 SUID/SGID 权限的程序时请勿使用 system()，因为 system() 会继承环境变量，通过环境变量可能会造成系统安全的问题
```
#include <stdlib.h>

int main(int argc, char *argv[])
{
	system(“ls -al /etc/passwd /etc/shadow”);
	return 0;
}
```

#### popen
popen() 会调用 fork() 产生子进程，然后从子进程中调用 /bin/sh -c 来执行参数 command 的指令。参数 type 可使用“r”代表读取，“w”代表写入。依照此 type 值，popen() 会建立管道连到子进程的标准输出设备或标准输入设备，然后返回一个文件指针。随后进程便可利用此文件指针来读取子进程的输出设备或是写入到子进程的标准输入设备中。此外，除了 fclose() 以外，其余所有使用文件指针（FILE *）操作的函数也都可以使用。


若成功则返回文件指针，否则返回 NULL，错误原因存于 errno中。
注意事项

在编写具 SUID/SGID 权限的程序时请尽量避免使用 popen()，因为 popen() 会继承环境变量，通过环境变量可能会造成系统安全的问题。

```
#include <stdio.h> 

int main(int argc, char *argv[])
{ 
	FILE *fp; 
	char buffer[80]; 
	
	fp=popen("cat /etc/passwd", "r"); 
	fgets(buffer,sizeof(buffer),fp); 
	printf("%s",buffer); 
	
	return 0;
}
```

#### exec 函数簇
```
int execl(const char *path, const char *arg, ...); 
int execlp(const char *file, const char *arg, ...);    
int execle(const char *path, const char *arg, ..., char *const envp[]);    
int execv(const char *path, char *const argv[]);    
int execvp(const char *file, char *const argv[]);    
int execve(const char *path, char *const argv[], char *const envp[]);
```

原文链接：https://blog.csdn.net/lu_embedded/article/details/78669939