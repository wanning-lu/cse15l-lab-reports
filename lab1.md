# Lab 1: Remotely accessing a server

When I began the first lab of the class, I was *incredibly* unfamiliar with operating systems, terms like "bash", and why we needed to use the terminal in VSCode as opposed to using our native terminal. Before starting the listed tasks below, it's helpful to understand what exactly you are doing.

## What are operating systems?
Let's familiarize ourselves with some common terms associated with operating systems:
- [Operating system][1]: Software that manages all of the software and hardware with the ability to run multiple processes at once (multitasking). Some of its functionalities include memory allocation, communicating with hardware peripherals (devices connected to the computer), and handling I/O.
- [Shell][2]: Program that allows for the user to interact with the operating system
- [Bash][3]: A Unix shell/command language
- [SSH][4]: A network communication protocol allowing for encrypted communication between two computers (remote access)

[1]: https://www.youtube.com/watch?v=26QPDBe-NB8
[2]: https://en.wikipedia.org/wiki/Shell_(computing)
[3]: https://en.wikipedia.org/wiki/Bash_(Unix_shell)#cite_note-:0-20
[4]: https://www.ucl.ac.uk/isd/what-ssh-and-how-do-i-use-it

## Installing Visual Studio Code
VSCode's integrated terminal allows for the user to switch between different shells with ease, unlike with the native terminal. In order to install VSCode, go to [this download link][11] and click the download button corresponding to your respective operating system. Afterwards, you should have a .zip link which you may unzip to install the program.

[11]: https://code.visualstudio.com/download

![VSCode download page](images/lab1-1.png)
*Download page for VSCode*

## Remotely connnecting
In this step, you will be getting remote access to the ieng6.ucsd.edu server running Linux. Open VSCode and its integrated terminal. Ensure that you are running Bash (Linux/macOS) or Git Bash (Windows). To login to the server, use the following command: 

```ssh [username]@ieng6.ucsd.edu``` 

with your course-specific username replacing [username]. You may find your username at [this link][10]. Afterwards, you'll be prompted to enter your password. Enter it as you would normally do so, but be aware of the fact that no characters will show up. Once you've entered your password, you will be asked for confirmation to continue connecting–respond by typing 'yes'. You should be met with the following screen:

[10]: https://sdacs.ucsd.edu/~icc/index.php

![Server connection screen](images/lab1-2.png)
*A successful connection*

Congrats! You have successfully connected to the remote server!

## Running Commands

Since Bash is the native shell for most Linux distributions, you should be able to use Bash commands on the remote server, which is a Linux system. We can use commands to navigate around the system, create directories/files, or retrieve information from files. Here's a list of several useful [Bash commands][5]:

| Command | Usage                                                         |
|---------|---------------------------------------------------------------|
| ```pwd```     | Prints the working directory you are in and its absolute path |
| ```ls```      | List contents of the directory                                |
| ```cd```      | Change the current working directory                          |
| ```touch```   | Create a file                                                 |
| ```mkdir```   | Create a directory                                            |
| ```rmdir```   | Delete a directory                                            |
| ```less```    | View contents of a text file                                  |
| ```cat```     | Read, create, or combine files                                |

We can start by using ```pwd```, which shows us the directory we are currently in on the remote server.
![Using pwd](images/lab1-3.png)

Now that we see we are in the "cs15lwi23aiu" directory, which is within the "cs15lwi23" 
directory and so on. Analyzing this further, we can see that this is our home directory
for our user login. Using ```ls``` allows us to see the contents within our home
directory, which contains a file called "hello.txt".
![Using ls](images/lab1-4.png)

To read the contents of the file, we can use ```cat```:
![Using cat](images/lab1-5.png)

Lastly, we can try moving into the parent directory using ```cd``` 
and listing out its contents:
![Using cd and ls](images/lab1-6.png)

Good job! You've managed to access a server remotely and execute several commands
on it through the terminal.

[5]: https://www.educative.io/blog/bash-shell-command-cheat-sheet