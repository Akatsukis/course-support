# Introduction to SSH & Working on UCR's Servers

In this second VSCode module, you will learn:

* How to use Secure Shell (SSH) to connect to a remote server (both through command line and through VSCode)
* How to navigate a Linux operating system using the command line
* How to setup and work on a remote server

## Connecting to the CS010B Server

For this class (and many future ones), a remote server will be used to host all labs. You will learn how to connect to a server using SSH by two methods, the command line, and through VSCode.

### Command Line

VSCode and other graphical interfaces won't be accessible all the time, so it is good to learn how to SSH into a remote server using just the terminal in case you ever need to. The method of SSH varies with operating system, so select whichever operating system you are on. Before we proceed, make sure that you have a CS account. If you **don't have one/aren't sure if you have one**, [go to this link](https://systems.engr.ucr.edu/createaccount) to set one up. If you **forgot your password**, then [go here](https://systems.engr.ucr.edu/policies/password).

<details>
<summary>Windows 10 or newer</summary>
Windows doesn't have SSH enabled by default, so before we can run it from the command prompt/Powershell (whichever you prefer), we need to enable OpenSSH.

1. Go to the Start Menu and search for "Add an Optional Feature"
2. Click "Add a feature"
3. Search for OpenSSH Server and install

![Installing OpenSSH on Windows](images/openssh.gif)

Now, open a command prompt (search "CMD" in the start menu) or a PowerShell terminal (search "PowerShell" in the start menu) and test whether or not this worked by running the following command:

``` ssh [your_cs_username]@cs010b.cs.ucr.edu ```

If you are having difficulty with this method, use the method outlined in "Older Versions of Windows".
</details>

<details>
<summary>Older Versions of Windows</summary>

In order to use SSH, you will have to install a program called [PuTTY](https://www.putty.org/). When you open PuTTY, there will be a box for a "Host Name", where you will input `[your_cs_username]@cs010b.cs.ucr.edu`. You can also use [Cygwin](cygwin.com) or [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) instead if you wish; these applications emulate a Linux environment in Windows.
</details>

<details>
<summary>Linux or MacOS</summary>
Run the following command in your terminal:
<pre>ssh [your_cs_username]@cs010b.cs.ucr.edu</pre>
Replace [your_cs_username] with the username provided to you by the CS department.
</details>

If you have successfully established an SSH connection with the server, you should see an output like this:

```
The authenticity of host 'cs010b.cs.ucr.edu (xxx.xxx.xx.xx)' can't be established.
ECDSA key fingerprint is SHA256:TASH&SAHw89h2389hASUIdl.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

> Note: This prompt appears every time you SSH into a server for the first time, so don't let it scare you! This happens because whenever you SSH into a server, there is a list of known hosts saved on your computer. Since you have (presumably) never SSH'ed into this server, then you will get this message only the first time since the address of the server will then be added to the list of known hosts.

Type "yes". You will be prompted to enter your CS password. As a security measure, no characters will be echoed into the console as you type your password. Once you have sucessfully logged in, you should be able to see that your terminal's user has changed to [your CS username] @ xe-02. You have sucessfully SSH'd into the UCR servers through the command line!

In order to terminate the SSH connection, use the `exit` command. **PLEASE DON'T FORGET TO DO THIS WHENEVER YOU ARE DONE!** Closing out of the terminal without using the `exit` doesn't log you out on the server, which consumes precious server resources.

> Note: You are NOT required to do every (or any) assignment on the remote servers, in fact we encourage you to do all the assignments in your own personal development environment. The reason we want to show you this method is that future classes will require you to use the servers in order to access the material (**and will assume you can do it**), so we are making sure you are familiar with the process now.

## Navigating the Linux File System

Navigating through the Linux file system will be a bit different because we don't have a graphical interface, we only have the terminal. However, the file system itself isn't too different from other file systems you may be familiar with. A helpful way to visualize the file system is to view it as a tree: a tree starts at the root. In this case, it would be the root directory which is denoted as `/` in Linux and usually as `C:\` in Windows. Within these root directories there are many files and directories that live within that root directory called children, and those children have children. We will go over several commands that you use to traverse the file system through the command line.

> Note: Even though we are only using command line without VSCode here, it is still important to understand how to navigate a file system with the terminal even if you have a graphical interface! One example is for knowing which files to compile and how to correctly navigate to them so you can tell the compiler where to look.

|![Tree of Linux File System](images/linuxfilesystem.png)|
|:--:|
| *A visual example of the Linux file system* |

Whenever you log into any of the school servers, you should be placed into your user root directory/home directory. In order to see the current directory you are in, use the `pwd` command (print working directory). The path printed should be `/home/csmajs/[your CS username]`. If not, use the `cd ~` command, where `cd` is the command to "change directory" and `~` is an alias that represents your personal home directory.

Now, lets use the file system. Type the following command:

``` mkdir example_dir ```

The `mkdir` command stands for "make directory". Since your current path was your home directory, this means your new directory was created here. To make sure, run the `ls` command (stands for "list"). Let's change into that new directory with `cd example_dir`. Our path now should be `/home/csmajs/[your CS username]/example_dir`. Type `pwd` just to make sure that this is your path, since we changed to the `example_dir` directory. Now, lets go back to the home directory. Type `cd ..` The `..` is an alias that represent the previous directory/the current directory's parent. (similary, as mentioned in module 1, a single period `.` represents the current directory). These aliases (`.`, `..`, and `~`) make it much easier to move through the file system without typing the full path and names of directories.

Now try typing the whole path.

```cd /home/csmajs/<your_CS_username>/example_dir```

Check that we are in `example_dir` with `pwd` again, but this time we typed the whole path instead of just `cd example_dir`. This is because, in the first case, we used a *relative* path, since we were changing directories relative to our current directory. The path we used this time was an *absolute* path, since we started with the root directory `/`. Paths that start with the root directory `/` are always absolute, otherwise they are relative.

Go back to your home directory (you should know how to do this by now). The next commands we will go over are `touch` and `mv`. `touch [filename]` is simple: it creates a file with that name. `mv` stands for move, and the parameters are `mv [source file] [target directory]`. It is used to move a file from one directory to another. If the "target directory" is actually a file name and not a directory, then it will simply rename the source file to that name. So, `mv` is used to both move and rename files. Let's create a file called `example.txt`, and move it into our example directory. We can do this with the following commands:

```
touch example.txt
mv example.txt example_dir
```

Another command we will cover is `rm`. Now that we know how to traverse the file system, lets delete our example directory. Make sure you're home (`pwd`) and type this command:

``` rm -rf example_dir ```

`rm` stands for "remove". If we were removing a normal file, we wouldn't need these flags, but because this is a directory, we need the `-rf` flags. `-r` means to remove files recursively (this means it deletes all files and directories within that directory, so `example.txt` will also be deleted), and `-f` disables user confirmation for each deletion.

> Note: `rm -rf` is crazy powerful. You can essentially break your whole system with `rm -rf /` (don't do this, you have been warned). So, be very careful when you use this command, as there is no undoing these deletions (notice there isn't any recycle bin).

To prepare for the next part, lets make some directories and some files. We are going to create two directories called `src` and `header`, and put some source code files in `src` and header files in `header`. Run these commands:

```
mkdir module2
cd module2
mkdir src
mkdir header
cd src
touch main.cpp
touch person.cpp
cd ../header
touch person.h
```

As a summary, these are the commands we went over (feel free to use this as a cheatsheet until you get used to navigating file systems via the command line):

* `pwd` prints the current working directory's path.
* `mkdir` makes a directory.
* `cd` changes your directory to the directory passed in by a path.
    * `cd ..` changes your directory to the parent directory (goes to previous directory).
* `ls` lists all files in your current directory.
* `touch` creates a file.
* `mv [source file] [destination directory]` moves a file from one place to another, and can also be used to rename a file.
* `rm` removes a file.
    * `rm -rf` removes a directory, and all files within it.

> Note: Bash has a built in manual that shows you how to use a command and all the possible flags for it. You can use the `man` command (stands for manual), and do something like `man cd` in order to see the instructions for the `cd` command. You can also use the `--help` flag for most commands.

## SSH with VSCode

Now, we won't make you edit files using the command line, so we're gonna switch gears to using SSH with VSCode. `exit` out of the SSH of the command line, close your terminal and open VSCode. Go to the extension marketplace, and search for Remote-SSH (the one published by Microsoft, with the blue ribbon verification). Install it. Now, we can SSH using VSCode! To set up to connect to CS010B servers, follow these steps:

1. Open the command palette (F1), and type "Remote-SSH". Select the "Remote-SSH: Connect to Host" option.
2. Click add a new host, and type the following command:

    ``` ssh [your_cs_username]@cs010b.cs.ucr.edu ```

3. Once you connect, another instance of VSCode will open, and ask you for your password.
4. Since we are connected to a remote server, we have to configure our extensions again. Go to the extensions marketplace, and install the C++ extension.

Now, you have a fully configured workspace on VSCode while connected to school servers!

In order to close the connection, click the bottom left corner (it should say `SSH: cs010b.cs.ucr.edu`). It should pull up the command palette, and to exit, press "Close remote connection".

## Working on a Remote Server

Everything you have done in the previous parts can now be done using the terminal or the graphical interface of VSCode, whichever you prefer.

Open a folder using Ctrl+O / Cmd+O. This should default to your home directory (the one using your cs username). Once you open it, you should see the directories and files you made earlier. We will be making a simple Person class to demonstrate how to work with multiple files and directories. In each of the following files, paste the following code into each respective file.

`src/main.cpp`

```cpp
#include <iostream>
#include <string>
#include "../header/person.h"
using namespace std;

int main()
{
    Person myPerson = Person("Bob");
    myPerson.printName();
    return 0;
}
```

`header/person.h`

```cpp
#ifndef PERSON_H
#define PERSON_H

#include <iostream>
#include <string>
using namespace std;

class Person
{
    public:
        Person();
        Person(string name);
        void printName();
    private:
        string name;
        void assignName(string name);
};

#endif
```

`src/person.cpp`

```cpp
#include "../header/person.h"

Person::Person()
{
    name = "";
}
Person::Person(string name)
{
    this->name = name;
}
void Person::printName()
{
    cout << name << endl;
}
void Person::assignName(string name)
{
    this->name = name;
}
```

Now, open a terminal using Ctrl + \` or Cmd + \`. By default, you should be in your home directory (if not, just do `cd ~`). In order to compile our program, we need to be able to navigate to our source files. So, the command would be:

``` g++ -o person src/main.cpp src/person.cpp ```

> Note: Since our terminal is connected to the server, we are using `g++` on the server, not your machine. If your install of `g++` or any other required software is not working for whatever reason, using the remote server is a good backup.

This command creates an executable called `person` targeting the source files `main.cpp` and `person.cpp` both contained in the `src` directory. Notice that we don't include `header/person.h` in the compilation command. We already included this file in the source files with `#include "../header/person.h"` (this is also an example of why header guards are important, as we included them in both source files).

In order to make things simpler, you can also first change to the `src` directory (`cd src`) and compile there, just note that your executable will be in the `src` directory as well.

> Note: A neat trick you can use to save time is when you compile, you can do `g++ src/*.cpp` or `g++ *.cpp` (if you are in the `src` directory) because Bash supports [regular expressions](https://en.wikipedia.org/wiki/Regular_expression). Keep in mind that this compiles **all** your source files together.

Also, take note of the `#include`. The point of header files is that we want to include them in our source files since the header files contain our declarations. In order to do that, we must have a relative path from our `src` directory to the `header` directory. In order to get to a file from the `src` directory to the `header` directory, we must go to the previous directory `..` (which is our home directory in this case), then we go into the `header` directory, and `#include` whatever file we need. It is important to understand how to traverse the file system in order to compile the right files together, and include the right files in our source files. Organizing your files and code in this manner allows you to stay organized and know where to look while you are developing.

Now that you're familiar with the Linux file system, SSH with the command line, setting up VSCode with your work and compilation with multiple files, you have everything you need to work on remote servers!