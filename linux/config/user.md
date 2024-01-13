# user

用户环境配置

## 配置
最好设置代理

```shell
<!-- <!-- 移除: 使用 snap --> -->
<!-- sudo add-apt-repository ppa:neovim-ppa/unstable -->
<!-- sudo apt-get install neovim -->
snap install nvim
snap install chezmoi --classic
<!-- curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun -->
snap install docker
<!-- sudo add-apt-repository ppa:longsleep/golang-backports  -->
<!-- sudo apt-get install golang-go -->
snap install go --classic
snap install node --classic
<!-- sudo apt-get install screenkey -->
sudo apt install screenkey

# 安装 edge

<!-- <!-- 移除: 使用 clash verge(开放源码) 替换 --> -->
<!-- # 安装 Clash 代理工具 -->
<!-- # 配置代理 -->

# 目录名修改
nvim ~/.config/user-dirs.dirs
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
sudo apt install curl git zsh tmux gcc llvm g++ gdb python3-pip xclip libglib2.0-dev libpixman-1-dev i3

# 设置默认 shell 程序
chsh --shell=/bin/zsh

# 安装 ohmyzsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# ssh 配置
git config --global user.name "black"
git config --global user.email "1358244533@qq.com"
ssh-keygen -t rsa -C "1358244533@qq.com"

chezmoi init git@github.com:MyBlackHole/Mackup.git --branch ubuntu
chezmoi apply

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

# tmux 配置
git clone https://github.com/gpakosz/.tmux.git $HOME/.tmux 
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .

# 切换 i3 环境
sudo update-alternatives --config x-session-manager 

# cargo
curl https://sh.rustup.rs -sSf | sh

# lvim (python 依赖不需要)
sudo apt install python3-pynvim
bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/fc6873809934917b470bff1b072171879899a36b/utils/installer/install.sh)

<!-- npm 需要 -->
sudo chown -R black:black /usr/local/{lib,bin,share}

<!-- # fonts -->
<!-- mkdir -p ~/.local/share/fonts -->
<!-- fc-cache -f -v -->
下载 DroidSansMono.zip 安装到 /usr/share/fonts/DroidSansMono (可以直接双击字体文件安装, 三个都需要)

<!-- 设置输入切换 -->
ibus-setup (中文输入法需要安装)
reboot


sudo apt install xorg lightdm lightdm-gtk-greeter i3-wm i3lock i3status i3blocks dmenu terminator rofi polybar mate-power-manager xserver-xorg-input-libinput

sudo systemctl enable lightdm.service
sudo systemctl start lightdm.service

sudo nvim /etc/X11/xorg.conf.d/70-synaptics.conf
Section "InputClass"
        Identifier "libinput touchpad catchall"
        Driver "libinput"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Option "Tapping" "on"
        Option "NaturralScrolling" "true"
        Option "ClickMethod" "clickfinger"
EndSection
```
