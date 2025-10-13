# Windows and WSL2

We will not cover step-by-step instructions for installing WSL2 on Windows, as the internet is full of guides.  
Even the official Windows website provides a step-by-step tutorial on how to install WSL2 on your machine.

<a href="https://learn.microsoft.com/en-us/windows/wsl/install" target="_blank">Official WSL2 Guide</a>

However, we will share some recommendations on how to use this tool to develop with Rocketdoo and VSCode.

Assuming you already have WSL2 installed on your machine, properly configured with sudo access for the user you created,  
let’s continue with the necessary steps to prepare your machine for development with Rocketdoo and VSCode.

Keep in mind that you can use any Linux distribution,  
although for daily use we recommend Ubuntu 20/22/24.04 or Debian 11/12.

## Required Steps:

1. Open the WSL2 terminal.
2. Update the Linux packages:

~~~ 
sudo apt update
~~~

3. Install Git:

~~~
sudo apt install git -y
~~~

4. Install Visual Studio Code inside WSL2:

~~~
sudo apt install code
~~~

5. After installing VSCode, you can open it with the command:

~~~
code .
~~~

6. Lastly, make sure that “pip” is installed. If it’s not, install it:

~~~
sudo apt install python-pip
~~~

* Now you can continue with the [Steps](steps.md)

***Once these tools are set up, you can continue with the steps on the Instructions page,  
as the execution process is the same from that point on.***
