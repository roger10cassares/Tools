# [  Android Studio  ]

## Install Android Studio IDE 4.2

*From: https://developer.android.com/studio?hl=pt&gclid=Cj0KCQjwp86EBhD7ARIsAFkgakjpRJh3fGte1FGVxY5mpg0JZhGlkc9xC1J7hR6tC6EfDWyLSiWHBHIaAtcoEALw_wcB&gclsrc=aw.ds*

Download the Linux x64 binarries at https://developer.android.com/studio?hl=pt&gclid=Cj0KCQjwp86EBhD7ARIsAFkgakjpRJh3fGte1FGVxY5mpg0JZhGlkc9xC1J7hR6tC6EfDWyLSiWHBHIaAtcoEALw_wcB&gclsrc=aw.ds website

Move the tar.gz file to the ~/Tools/Android/ directory
```bash
mv ~/Downloads/android-studio-ide-202.7322048-linux.tar.gz ~/Tools/Android/
```

Uncrompress the android-studio-ide-202.7322048-linux.tar.gz file at the ~/Tools/Android/ directory
```bash
cd ~/Tools/Android/
tar -vxf android-studio-ide-202.7322048-linux.tar.gz
```

Open the ~/.bashrc file and include the android-studio binaries in the environment variable called PATH in the last lines of this file.

1. Open the .bashrc file, that is the file that configure the Linux Terminal default options.
```bash
nano ~/.bashrc
```


2. Then, add in the last line of the file:
```bash
# Set android-studio binaries to the PATH
export PATH=$PATH:~/Tools/Android/android-studio/bin
export ANDROID_HOME=~/Android/android-sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

3. Save and Exit the file editor with Ctrl^O, ENTER, Ctrl^X

4. If the Terminal is open, the Terminal`s configuration file must be reload.
```bash
source ~/.bashrc
```

5. (SKIP Step 4) Closing the Terminal with Ctrl^D or exit command or just closing then reopening, the changes shall be applied also.


Validate the installation just verifying if java commands are available from the Terminal

```bash
studio.sh
```



![image-20210506154729611](/home/roger/.config/Typora/typora-user-images/image-20210506154729611.png)





![image-20210506154918511](/home/roger/.config/Typora/typora-user-images/image-20210506154918511.png)



![image-20210506155357788](/home/roger/.config/Typora/typora-user-images/image-20210506155357788.png)





![image-20210506155421863](/home/roger/.config/Typora/typora-user-images/image-20210506155421863.png)





![image-20210506155457776](/home/roger/.config/Typora/typora-user-images/image-20210506155457776.png)







![image-20210506155549163](/home/roger/.config/Typora/typora-user-images/image-20210506155549163.png)





![image-20210506155631476](/home/roger/.config/Typora/typora-user-images/image-20210506155631476.png





![image-20210506155709515](/home/roger/.config/Typora/typora-user-images/image-20210506155709515.png



![image-20210506155819579](/home/roger/.config/Typora/typora-user-images/image-20210506155819579.png)





![image-20210506155858360](/home/roger/.config/Typora/typora-user-images/image-20210506155858360.png)











![image-20210506155920331](/home/roger/.config/Typora/typora-user-images/image-20210506155920331.png)







![image-20210506155942993](/home/roger/.config/Typora/typora-user-images/image-20210506155942993.png)





![image-20210506163747683](/home/roger/.config/Typora/typora-user-images/image-20210506163747683.png)



![image-20210506170456013](/home/roger/.config/Typora/typora-user-images/image-20210506170456013.png)





![image-20210506170633560](/home/roger/.config/Typora/typora-user-images/image-20210506170633560.png)













![image-20210506171816055](/home/roger/.config/Typora/typora-user-images/image-20210506171816055.png)









![image-20210506171619214](/home/roger/.config/Typora/typora-user-images/image-20210506171619214.png)





![image-20210506171642664](/home/roger/.config/Typora/typora-user-images/image-20210506171642664.png)





![image-20210506173542349](/home/roger/.config/Typora/typora-user-images/image-20210506173542349.png)





![image-20210506173551132](/home/roger/.config/Typora/typora-user-images/image-20210506173551132.png)















![image-20210506173458131](/home/roger/.config/Typora/typora-user-images/image-20210506173458131.png)







**![image-20210506173516248](/home/roger/.config/Typora/typora-user-images/image-20210506173516248.png)**





![image-20210506173527196](/home/roger/.config/Typora/typora-user-images/image-20210506173527196.png)







![image-20210506173604739](/home/roger/.config/Typora/typora-user-images/image-20210506173604739.png)











![image-20210506173614704](/home/roger/.config/Typora/typora-user-images/image-20210506173614704.png)











Thats it! The system is ready to use Java JDK commands!