From: https://react-native.rocketseat.dev/android/linux

## Prerequisites

To configure the Android Envioronment, olease install the softwares listed according the instuctions bellow:

  * Node.js 14 (LTS);
  * Yarn 1;
  * JDK 11 (LTS);
  * Android Studio e dependências.

Note: The global installation of React Native shall not be used. Instead, the command to create a new Project the commands shall be executed with npx.



Instalando libs gráficas
Em grande parte das vezes precisamos instalar algumas bibliotecas da versão 32 bits do Linux para conseguir emular nosso projeto. Por isso, vamos utilizar o seguinte comando:

sudo apt-get install gcc-multilib lib32z1 lib32stdc++6