# [  Open Java SE Development Kit 11.0.11 ]

## Install Open JDK 11

*From: https://jdk.java.net/java-se-ri/11*

Download the Linux x64 binarries at https://jdk.java.net/java-se-ri/11 website

Move the tar.gz file to the ~/Tools/Java/OpenJDK directory
```bash
mv ~/Downloads/openjdk-11+28_linux-x64_bin.tar.gz ~/Tools/Java/OpenJDK
```

Uncrompress the openjdk-11+28_linux-x64_bin.tar.gz file at the ~/Tools/Java/OpenJDK directory
```bash
cd ~/Tools/Java/OpenJDK
tar -vxf openjdk-11+28_linux-x64_bin.tar.gz
```

Open the ~/.bashrc file and include the Java JDK binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.
```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:
```bash
# Set Java OpenJDK binaries to the PATH
export PATH=$PATH:~/Tools/Java/OpenJDK/jdk-11/bin
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.
```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if java commands are available from the Terminal

```bash
java -version
----
openjdk version "11" 2018-09-25
OpenJDK Runtime Environment 18.9 (build 11+28)
OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)
----
```

## 

Thats it! The system is ready to use Java JDK commands!