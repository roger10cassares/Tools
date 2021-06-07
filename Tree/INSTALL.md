# [  Tree ]

## Install Tree

*From: https://www.linuxfromscratch.org/blfs/view/svn/general/tree.html*

Download the Linux x64 binarries at https://www.linuxfromscratch.org/blfs/view/svn/general/tree.html website

Move the tar.xz file to the ~/Tools/Tree directory

```bash
mv ~/Downloads/tree-1.8.0.tgz ~/Tools/Tree
```

Uncrompress the tree-1.8.0.tgz file at the ~/Tools/Tree directory. After that, compile with make command.

```bash
cd ~/Tools/Tree
tar -vzxf tree-1.8.0.tgz
cd tree-1.8.0
make
```

Open the ~/.bashrc file and include the Tree binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.

```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:

```bash
# Set Tree to the PATH
export PATH=$PATH:~/Tools/Tree/tree-1.8.0
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.

```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if node and npm commands are avaiable from the Terminal

```bash
tree --version
----
tree v1.8.0 (c) 1996 - 2018 by Steve Baker, Thomas Moore, Francesc Rocher, Florian Sesser, Kyosuke Tokoro 
----
```



Thats it! The system is ready to use tree commands!

