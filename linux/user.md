# 

## 配置
最好设置代理

```shell
sudo add-apt-repository ppa:neovim-ppa/unstable
sudo apt-get install neovim

# 安装 edge

# 安装 Clash 代理工具
# 配置代理

# 目录名修改
lvim ~/.config/user-dirs.dirs
# 修改
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"

# 基本工具
sudo apt install curl npm git zsh tmux gcc llvm g++ golang gdb python3-pip xclip libglib2.0-dev libpixman-1-dev i3 black

# 切换 i3 环境
sudo update-alternatives --config x-session-manager 

# npm 需要
sudo chown -R black:black /usr/local/{lib,bin,share}

# 设置默认 shell 程序
chsh --shell=/bin/zsh

# 安装 ohmyzsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# ssh 配置
git config --global user.name "black"
git config --global user.email "1358244533@qq.com"
ssh-keygen -t rsa -C "1358244533@qq.com"

# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions

# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# zsh-history-substring-search
git clone https://github.com/zsh-users/zsh-history-substring-search.git $ZSH_CUSTOM/plugins/zsh-history-substring-search

# powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
nvim ~/.zshrc
## 修改 ZSH_THEME
ZSH_THEME="powerlevel10k/powerlevel10k"
source ~/.zshrc

# yarn
npm install -g yarn

# tmux 配置
git clone https://github.com/gpakosz/.tmux.git $HOME/.tmux 
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .

# fd
wget https://github.com/sharkdp/fd/releases/download/v8.7.0/fd_8.7.0_amd64.deb
sudo dpkg -i fd_8.7.0_amd64.deb
rm fd_8.7.0_amd64.deb

npm install -g n

# mackup 同步配置工具
pip install --user mackup

# cargo
curl https://sh.rustup.rs -sSf | sh

# lvim
bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/fc6873809934917b470bff1b072171879899a36b/utils/installer/install.sh)
```
