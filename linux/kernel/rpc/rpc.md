# rpc

## install
```shell
sudo apt-get install rpcbind
```

```shell
systemctl status rpcbind.socket
● rpcbind.socket - RPCbind Server Activation Socket
     Loaded: loaded (/lib/systemd/system/rpcbind.socket; disabled; preset: enabled)
     Active: active (running) since Wed 2023-06-07 22:44:02 CST; 7h ago
      Until: Wed 2023-06-07 22:44:02 CST; 7h ago
   Triggers: ● rpcbind.service
     Listen: /run/rpcbind.sock (Stream)
             0.0.0.0:111 (Stream)
             0.0.0.0:111 (Datagram)
             [::]:111 (Stream)
             [::]:111 (Datagram)
      Tasks: 0 (limit: 18298)
     Memory: 16.0K
        CPU: 3ms
     CGroup: /system.slice/rpcbind.socket

6月 07 22:44:02 black systemd[1]: Listening on rpcbind.socket - RPCbind Server Activation Socket.
```
