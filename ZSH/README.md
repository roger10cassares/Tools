# [  ZSH ]

## Install ZSH


```bash
sudo apt-get update

# install zsh
sudo apt install zsh

# check zsh installed
zsh --version
---
zsh 5.8 (x86_64-ubuntu-linux-gnu)

# Configure the zsh terminal as the dafault system shell
chsh -s $(which zsh)
```

At this moment, please logout and login again in your user account

OPenninf the terminal again shall appears the information of the new zsh terminal

```shell
This is the Z Shell configuration function for new users,
zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.

You can:

(q)  Quit and do nothing.  The function will be run again next time.

(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.

(1)  Continue to the main menu.

(2)  Populate your ~/.zshrc with the configuration recommended
     by the system administrator and exit (you will need to edit
     the file by hand, if so desired).

--- Type one of the keys in parentheses --- 
```



Verify that is all ok typing 

```shell
echo $SHELL
---
/usr/bin/zsh
```



## Oh My Zsh

Install Oh My Zsh Framework

```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
---
Cloning into '/home/roger/.oh-my-zsh'...
remote: Enumerating objects: 1239, done.
remote: Counting objects: 100% (1239/1239), done.
remote: Compressing objects: 100% (1204/1204), done.
remote: Total 1239 (delta 20), reused 1103 (delta 15), pack-reused 0
Receiving objects: 100% (1239/1239), 863.71 KiB | 4.00 MiB/s, done.
Resolving deltas: 100% (20/20), done.

Looking for an existing zsh config...
Using the Oh My Zsh template file and adding it to ~/.zshrc.

         __                                     __
  ____  / /_     ____ ___  __  __   ____  _____/ /_
 / __ \/ __ \   / __ `__ \/ / / /  /_  / / ___/ __ \
/ /_/ / / / /  / / / / / / /_/ /    / /_(__  ) / / /
\____/_/ /_/  /_/ /_/ /_/\__, /    /___/____/_/ /_/
                        /____/                       ....is now installed!


Before you scream Oh My Zsh! please look over the ~/.zshrc file to select plugins, themes, and options.

• Follow us on Twitter: https://twitter.com/ohmyzsh
• Join our Discord server: https://discord.gg/ohmyzsh
• Get stickers, shirts, coffee mugs and other swag: https://shop.planetargon.com/collections/oh-my-zsh

➜  ~ 

```



Fira Code Font

```shell
sudo apt install fonts-firacode fontconfig

fc-list | grep FiraCode
---
/usr/share/fonts/opentype/firacode/FiraCode-Retina.otf: Fira Code,Fira Code Retina:style=Retina,Regular
/usr/share/fonts/opentype/firacode/FiraCode-Bold.otf: Fira Code:style=Bold
/usr/share/fonts/opentype/firacode/FiraCode-Regular.otf: Fira Code:style=Regular
/usr/share/fonts/opentype/firacode/FiraCode-Medium.otf: Fira Code,Fira Code Medium:style=Medium,Regular
/usr/share/fonts/opentype/firacode/FiraCode-Light.otf: Fira Code,Fira Code Light:style=Light,Regular
```



Spaceship Theme (Oh My Zsh)

```shell
cd ~/Tools/ZSH
git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"

echo $ZSH_CUSTOM
/home/roger/.oh-my-zsh/custom

ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"



```

Inside ~/.zshrc, modify ZSH_THEME to 



```shell
ZSH_THEME="spaceship"
```

Close and open the terminal to apply tha changes



Configuriong Spaceship

Disableing some optons

In the end of `'.zshrc file add the following:

```shell
SPACESHIP_PROMPT_ORDER=(
  user          # Username section
  dir           # Current directory section
  host          # Hostname section
  git           # Git section (git_branch + git_status)
  hg            # Mercurial section (hg_branch  + hg_status)
  exec_time     # Execution time
  line_sep      # Line break
  vi_mode       # Vi-mode indicator
  jobs          # Background jobs indicator
  exit_code     # Exit code section
  char          # Prompt character
)
SPACESHIP_USER_SHOW=always
SPACESHIP_PROMPT_ADD_NEWLINE=false
SPACESHIP_CHAR_SYMBOL="❯"
SPACESHIP_CHAR_SUFFIX=" "
```



Close and open the terminal to apply tha changes



Zinit (Plugins Zsh)

ZSH package manager

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/zdharma/zinit/master/doc/install.sh)"
```

Press Y to install the recommended plugins



Após a instalação, abra o arquivo ~/.zshrc novamente e abaixo da linha ### End of Zinit's installer chunk, que foi adicionada automaticamente no arquivo, adicione:



```shell
### Zinit Plugins
zinit light zdharma/fast-syntax-highlighting
zinit light zsh-users/zsh-autosuggestions
zinit light zsh-users/zsh-completions
```



- `zdharma/fast-syntax-highlighting`: Adiciona syntax highlighting na hora da escrita de comandos, facilitando principalmente o reconhecimento de comandos que foram digitados de forma incorreta;
- `zsh-users/zsh-autosuggestions`: Sugere comandos baseados no histórico de execução conforme você vai digitando;
- `zsh-users/zsh-completions`: Adiciona milhares de *completitions* para ferramentas comuns como Yarn, Homebrew, NVM, Node, etc, para você precisar apenas apertar TAB para completar comandos;



Close and open the terminal to apply tha changes para instalar os plugins automaticamente.







![image-20210629231109586](/home/roger/.config/Typora/typora-user-images/image-20210629231109586.png)





