Download binaries at https://go.dev/dl/ to src

```bash
cd src
wget https://go.dev/dl/go1.18.1.linux-amd64.tar.gz
```

Then, from src dir, extract to the dir level before
```bash

tar -C ../ -xzf go1.18.1.linux-amd64.tar.gz
```

After all, edit the .bashrc or .zshrc file, close and re-open the terminal
```bash
# Golang
export PATH=$PATH:~/Tools/Golang/go/bin
export GOPATH=~/Softwares/Golang/go
export GOCACHE=~/Softwares/Golang/go/.cache/go-build
export GOENV=~/Softwares/Golang/go/.config/go/env
```
