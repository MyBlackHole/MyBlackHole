# emulator

```shell
yum install centos-release-scl-rh centos-release-scl
yum --enablerepo=centos-sclo-rh  install rh-ruby26
/opt/rh/rh-ruby26/root/bin/gem sources --add https://mirrors.tuna.tsinghua.edu.cn/rubygems/ --remove https://rubygems.org/
/opt/rh/rh-ruby26/root/bin/gem install thor builder
/opt/rh/rh-ruby26/root/bin/ruby ./bin/emulator -r store -p 80

<!-- <!-- yum install ruby --> -->

gpg2 --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
/usr/local/rvm/bin/rvm install ruby-2.7

sudo gem sources --add https://mirrors.tuna.tsinghua.edu.cn/rubygems/ --remove https://rubygems.org/
sudo gem install thor builder
sudo ./bin/emulator -r store -p 80
```
