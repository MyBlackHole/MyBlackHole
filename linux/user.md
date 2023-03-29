# 

```shell
sudo add-apt-repository ppa:longsleep/golang-backports 
sudo apt-get install golang-go
sudo apt-get install screenkey

# fonts
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
fc-cache -f -v 
reboot

sudo apt install ibus ibus-libpinyin ibus-clutter ibus-gtk im-config
im-config -s ibus
ibus-setup
reboot

# docker
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
