# [  Chromium ]

## Install Chromium

*From: https://ungoogled-software.github.io/ungoogled-chromium-binaries/releases/linux_portable/64bit*

Download the Linux x64 binarries at https://ungoogled-software.github.io/ungoogled-chromium-binaries/releases/linux_portable/64bit website

Move the tar.xz file to the ~/Tools/Chromium directory

```bash
mv ~/Downloads/ungoogled-chromium_87.0.4280.141-1.1_linux.tar.xz ~/Tools/Chromium
```

Uncrompress the ffmpeg-release-amd64-static.tar.xz file at the ~/Tools/Chromiumeg directory

```bash
cd ~/Tools/Chromium
tar -vxf ungoogled-chromium_87.0.4280.141-1.1_linux.tar.xz
```

Open the ~/.bashrc file and include the FFmpeg binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.

```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:

```bash
# Set Chromium to the PATH
export PATH=$PATH:~/Tools/Chromium/ungoogled-chromium_87.0.4280.141-1.1_linux
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.

```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if node and npm commands are avaiable from the Terminal

```bash
fchrome --version
----
Chromium 87.0.4280.141
----
```



Thats it! The system is ready to use chrome commands!

