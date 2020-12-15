```bash

```

# Setting terminal config

* In bash/zsh config file

```bash

function proxy_on(){
    export http_proxy=http://127.0.0.1:7890
    export https_proxy=http://127.0.0.1:7890
    echo -e "Open proxy"
}
function proxy_off(){
    unset http_proxy
    unset https_proxy
    echo -e "close proxy"
}
```

Substitute 7890 with your proxy port. 



# Installing neovim



* MacOS

```bash
brew install neovim
```

* Arch

```bash
sudo pacman -S neovim
```



# Create config

Make directory for your Neovim config

```bash
mkdir ~/.config/nvim
```



Create an `init.vim` file

```bash
touch ~/.config/nvim/init.vim
```



# Install vim-plug





# add a new file for plugins

```bash
m
```







CocCommand

py setInterpreter



