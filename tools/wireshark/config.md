# config

```shell
<!-- sudo setcap cap_net_raw,cap_net_admin+eip /usr/bin/dumpcap -->


❯ sudo groupadd wireshark
groupadd：“wireshark”组已存在
❯ sudo chgrp wireshark /usr/bin/dumpcap
❯ sudo chmod 4755 /usr/bin/dumpcap
❯ sudo gpasswd -a black wireshark
正在将用户“black”加入到“wireshark”组中
```
