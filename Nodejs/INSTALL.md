# [ Install Nodejs, npm and yarn ]

## Install Nodejs and npm

*From: https://nodejs.org/en/download/*

Download the Linux x64 binarries at https://nodejs.org/en/download/ website

Move the tar.xz file to the ~/Tools/Nodejs directory
```bash
mv ~/Downloads/node-v14.16.1-linux-x64.tar.xz ~/Tools/Nodejs
```

Uncrompress the node-v14.16.1-linux-x64.tar.xz file at the ~/Tools/Nodejs directory
```bash
cd ~/Tools/Nodejs
tar -vxf node-v14.16.1-linux-x64.tar.xz
```

Open the ~/.bashrc file and include the Nodejs binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.
```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:
```bash
# Set Nodejs to the PATH
export PATH=$PATH:~/Tools/Nodejs/node-v14.16.1-linux-x64/bin
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.
```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if node and npm commands are avaiable from the Terminal

```bash
node -v
----
v14.16.1
----
```

```bash
npm -v
----
6.14.12
----
```



## Install Install Nodejs with npm

*From: https://classic.yarnpkg.com/en/docs/install/#debian-stable*

There is a recommendation to install Yarn through the npm package manager, which comes bundled with Node.js when is is install it on the system.

Once npm is installed, please run the following both to install and upgrade Yarn:

```bash
npm install --global yarn
```

Check the installation by running

```bash
yarn -v
----
1.22.10
----
```



Thats it! The system is ready to use node and npm commands!