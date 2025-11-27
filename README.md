# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:
```
DEVELOPED BY:kamalesh v
REG.NO:212222240042
```
## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2];  
    pid_t pid;
    char write_msg[] = "Hello from parent process!";
    char read_msg[100];

    
    if (pipe(fd) == -1) {
        perror("pipe failed");
        exit(EXIT_FAILURE);
    }

  
    pid = fork();

    if (pid < 0) {
        perror("fork failed");
        exit(EXIT_FAILURE);
    }

    if (pid > 0) {
        // Parent process
        close(fd[0]);  

        printf("Parent: Sending message to child...\n");
        write(fd[1], write_msg, strlen(write_msg) + 1);  

        close(fd[1]);  
    } 
    else {
       
        close(fd[1]);  

        read(fd[0], read_msg, sizeof(read_msg));
        printf("Child: Received message -> %s\n", read_msg);

        close(fd[0]);  
    }

    return 0;
}

```


## OUTPUT<img width="648" height="106" alt="1" src="https://github.com/user-attachments/assets/0ce34e56-2331-4d71-9f8f-3f3ba22740be" />




## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>

#define FIFO_PATH "/tmp/myfifo"

int main() {
    int fd;
    pid_t pid;
    char write_msg[] = "Hello from parent via named pipe!";
    char read_msg[100];

    
    if (mkfifo(FIFO_PATH, 0666) == -1) {
        perror("mkfifo");
    
    }

    pid = fork();

    if (pid < 0) {
        perror("fork");
        exit(EXIT_FAILURE);
    }

    if (pid > 0) {
      
        sleep(1);
        fd = open(FIFO_PATH, O_WRONLY);
        if (fd == -1) {
            perror("open for write");
            exit(EXIT_FAILURE);
        }

        printf("Parent: Writing to FIFO...\n");
        write(fd, write_msg, strlen(write_msg) + 1);
        close(fd);

        
        unlink(FIFO_PATH);
    } else {
     
        fd = open(FIFO_PATH, O_RDONLY);
        if (fd == -1) {
            perror("open for read");
            exit(EXIT_FAILURE);
        }

        read(fd, read_msg, sizeof(read_msg));
        printf("Child: Received -> %s\n", read_msg);
        close(fd);
    }

    return 0;
}


```




## OUTPUT
<img width="718" height="130" alt="2" src="https://github.com/user-attachments/assets/0a591b6c-3b74-4efe-a322-15e4c925415f" />



# RESULT:
The program is executed successfully.
