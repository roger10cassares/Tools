# [  VS Code ]

## Install VS Code

*From: https://code.visualstudio.com/docs/?dv=linux64*

Download the Linux x64 binarries at https://code.visualstudio.com/docs/?dv=linux64 website

Move the tar.xz file to the ~/Tools/VSCode directory

```bash
mv ~/Downloads/code-stable-x64-1620296658 ~/Tools/VSCode
```

Uncrompress the code-stable-x64-1620296658 file at the ~/Tools/VSCode directory

```bash
cd ~/Tools/VSCode
tar -vxf ncode-stable-x64-1620296658
```

Open the ~/.bashrc file and include the VSCode binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.

```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:

```bash
# Set Nodejs to the PATH
export PATH=$PATH:~/Tools/VSCode/VSCode-linux-x64/bin
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.

```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if node and npm commands are avaiable from the Terminal

```bash
code -v
----
1.56.1
e713fe9b05fc24facbec8f34fb1017133858842b
x64
----
```



Thats it! The system is ready to use code commands!

