# linux_laboratory
Labs and Assignment works of Linux Laboratory
# WEEK6

### 1.Write a C program to emulate the Unix ls-l command.
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>

int main() {
  int pid; // process id
  pid = fork(); // create another process
  if (pid < 0) { // fail
    printf("Fork failed\n");
    exit(-1);
  } else if (pid == 0) { // child
    execlp("/bin/ls", "ls", "-l", NULL); // execute ls
  } else { // parent
    wait(NULL); // wait for child
    printf("\nchild complete\n");
    exit(0);
  }
}
o/p :
 list the all detailed information of files through ls command
 
### 2.Write a C program to list for every file in a directory, itsinode number and fill name.
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) {
    char d[50];
    
    if (argc == 2) {
        memset(d, 0, sizeof(d));
        strcat(d, "ls -i ");
        strcat(d, argv[1]);
        system(d);
    } else {
        printf("\nInvalid number of inputs\n");
    }

    return 0;
}

### 3. Write a C Program that demonstrates redirection of standard output to a file. EX:: ls&gt;f1.
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[])
{
    char d[50];

    if (argc == 2)
    {
        // Use snprintf to safely concatenate strings
        snprintf(d, sizeof(d), "ls > %s", argv[1]);

        // Use system call with the formatted string
        system(d);
    }
    else
    {
        printf("\nInvalid No. of inputs");
    }

    return 0;
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


