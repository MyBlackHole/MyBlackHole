# nmcli

## 选项
### networking
查询NetworkManager状态，启用禁用网络（整个网络）

| 命令  |  作用 |
|---|---|
|语法|nmcli networking { on \| off \| connectivity } [ARGUMENTS...]|
|nmcli networking off|禁用所有网络|
|nmcli networking connectivity check|参数check表示重新检查连接状态。连接状态full表示具有完全的internet访问能力，limited表示连接到一个网络，但是internet未接入|
|nmcli networking on|启用网络|

### radio
显示无线电开关状态，启用、禁用无线电

|   |   |
|---|---|
|语法|nmcli radio { all \| wifi \| wwan } [ARGUMENTS...]|
|nmcli radio wifi|打印wifi开关状态|
|nmcli radio wifi off|表示关闭wifi|

### connection
NetworkManager会把网络配置保存为connections配置信息,比如保存二层网络信息，ip信息等。NetworkManger会根据这些信息知道怎么去连接一个网络。在一个特定设备，可以有多个连接配置(比如一个是dhcp的，一个是静态ip地址的)，但是只有一个配置是“活动“的配置。connection对象就是用来管理这些连接配置的

#### 语法
nmcli connection { show \| up \| down \| modify \| add \| edit \| clone \| delete \| monitor \| reload \| load \| import \| export } [ARGUMENTS...]<br><br>激活连接的语法详述：<br><br>语法： nmcli connection up [ id \| uuid \| path ] ID [ifname ifname] [ap BSSID] [passwd-file file]<br><br>解释：一个连接由其name(名称)，uuid或path(就是D-Bus路径)所标识。如果ID不明确，则可以使用关键字id，uuid或path来指明这个ID是什么。当需要特定设备激活连接时，应提供带有接口名称的ifname选项。如果未提供ID，则需要一个ifname，NetworkManager将为给定的ifname激活最佳的可用连接。如果是VPN连接，则ifname选项指定基本连接的设备。ap选项指定在Wi-Fi连接的情况下应使用哪个特定的AP。passwd-file:某些网络在激活期间可能需要凭据。您可以使用此选项提供这些凭据。文件的每一行应包含一个密码。文件内容格式为setting_name.property_name:the password。例如WPA-PSK连接的格式为802-11-wireless-security.psk:secret12345。nmcli还接受wifi-sec和wifi字符串，而不是802-11-wireless-security。当NetworkManager需要密码但未提供密码时，nmcli在使用--ask运行时会要求输入密码。如果未传递--ask，则NetworkManager可以询问可能正在运行的另一个秘密代理（通常是GUI秘密代理，例如nm-applet或gnome-shell）。<br><br>停用连接的语法详述：<br><br>语法: nmcli connection down [ id \| uuid \| path \| apath ] ID... #注意这里的ID可指定多个<br><br>解释: 请注意，该命令将停用指定的活动连接，但是该连接处于活动状态的设备仍可以连接，并且将通过查找设置了“自动连接”标志的合适连接来执行自动激活。 请注意，停用的连接配置文件在内部被阻止再次自动连接。 因此，它不会自动连接，直到重新启动或直到用户执行取消自动连接的操作为止，例如修改配置文件或显式激活它。在大多数情况下，您可能想使用device disconnect命令。<br><br>该连接由其名称，UUID或D-Bus路径标识。 如果ID不明确，则可以使用关键字id，uuid，path或apath。<br><br>修改连接的语法详述：<br><br>语法 : modify [--temporary] [ id \| uuid \| path ] ID { option value \| [+\|-]setting.property value } ...<br><br>解释: 在连接配置文件中添加，修改或删除属性。要设置属性，只需指定属性名称后跟值即可。空值（“”）将属性值重置为默认值。除了属性外，您还可以对某些属性使用简称。有关详细信息，请参阅“属性别名”部分。如果要将项目或标志附加到现有值，请使用+前缀作为属性名称或别名。如果要从容器类型或标志属性中删除项目，请使用-前缀。对于某些属性，您还可以通过指定从零开始的索引来删除元素。 +和-修饰符仅对支持它们的属性有效。例如，这些是多值（容器）属性或标志，例如ipv4.dns，ip4，ipv4.addresses，bond.options，802-1x.phase1-auth-flag等。有关设置和属性名称，其描述和默认值的完整参考，请参见nm-settings（5）。如果设置和属性是唯一的，则可以缩写。该连接由其名称，UUID或D-Bus路径标识。如果ID不明确，则可以使用关键字id，uuid或path。

|   |   |
|---|---|
|nmcli connection show|列出网络连接的配置(存放于内存和硬盘的配置，nmcli -f active connection show 表示显示存储于内存配置， -f profile表示存放于硬盘的配置)|
|nmcli connection show --active|仅列出处于活动状态的网络配置|
|nmcli --show-secrets -f  802-11-wireless-security.psk  connection show  myAP001|显示myAP001密码，加了--show-secrets或-s才能显示密码明文|
|nmcli connection show --order name|按配置名排序，可选排序有type、active、name、path(d-bus路径)，+号和-号表示升序和降序，未指定，则默认使用升序。默认排序是：--order active:name:path|
|nmcli connection show uuid 38781e62-4bab-4ba8-a086-bfaece222794|按指定关键字显示，关键字有id，uuid、path、apath。 用途是不能使用常规的nmcli connection show <配置名> 来显示的时候，这种显示方法就可以派上用场了|
|nmcli connection up prof1|激活一个连接。|
|nmcli connection down prof1|停用一个连接|
|nmcli connection delete prof1|删除一个配置， delete [ id \| uuid \| path ] ID...|
|nmcli connection add type ethernet ifname enp5s0|创建一个连接。这里没有指定method，则默认使用auto，也就是自动配置。类型是以太网，类型有以太网、wifi，adsl等，具体参考文章头部给的url|
|nmcli connection add ifname enp5s0 autoconnect yes type ethernet ip4 10.1.1.1/8 gw4 10.1.0.1|创建一个静态ip的以太网连接(自动连接)|
|nmcli connection modify myEth +ipv4.dns 8.8.8.8|给myEth的配置添加dns|
|nmcli connection modify myEth ipv4.method manual ipv4.addresses "192.168.43.64/24,10.0.0.23/8"|修改myEth连接为手动，ip地址设置为<br><br>两个|
|nmcli con mod myEth  autoconnect no|设置myEth连接配置为不自动连接(重启操作系统或从起NetworkManager就能看到不会自动连接了)|

### device
显示或管理网络接口

#### 语法
nmcli device { status \| show \| set \| connect \| reapply \| modify \| disconnect \| delete \| monitor \| wifi \| lldp } [ARGUMENTS...]<br><br>子命令详细解释(这里不全部翻译，理解了上面connection，这里也是差不多的)：<br><br>set [ifname] ifname [ autoconnect { yes \| no } ] [ managed { yes \| no } ]<br><br>Set device properties. connect ifname Connect the device. NetworkManager will try to find a suitable connection that will be activated. It will also consider connections that are not set to auto connect.If no compatible connection exists, a new profile with default settings will be created and activated. This differentiates nmcli connection up ifname "$DEVICE" from nmcli device connect "$DEVICE" If --wait option is not specified, the default timeout will be 90 seconds.<br><br>reapply ifname<br><br>Attempt to update device with changes to the currently active connection made since it was last applied.<br><br>modify ifname { option value \| [+\|-]setting.property value } ...<br><br>Modify the settings currently active on the device.<br><br>This command lets you do temporary changes to a configuration active on a particular device. The changes are not preserved in the connection profile.See nm-settings(5) for the list of available properties. Please note that some properties can't be changed on an already connected device.You can also use the aliases described in Property Aliases section. The syntax is the same as of the nmcli connection modify command.<br><br>disconnect ifname...<br><br>Disconnect a device and prevent the device from automatically activating further connections without user/manual intervention. Note that disconnecting software devices may mean that the devices will disappear.(这段翻译一下：断开设备连接，并防止设备自动激活。 断开软设备的连接可能会让这些设备消失)<br><br>If --wait option is not specified, the default timeout will be 10 seconds.<br><br>delete ifname...<br><br>Delete a device. The command removes the interface from the system. Note that this only works for software devices like bonds, bridges, teams, etc(仅能删除软设备，例如bond、桥等). Hardware devices (like Ethernet) cannot be deleted by the command.（硬设备无法删除）<br><br>wifi [ list [--rescan \| auto \| no \| yes ] [ifname ifname] [bssid BSSID] ]<br><br>列出wifi热点，可以指定无线网卡接口名和热点BSSID。列出的热点列表不会超过30秒，必要时会重新扫描(通过--rescan控制重新扫描：取值auto yes no)<br><br>wifi connect (B)SSID [password password] [ifname ifname] [bssid BSSID] [name name] [ private { yes \| no } ] [ hidden { yes \| no } ]<br><br>连接到由SSID或BSSID指定的wifi网络。该命令找到匹配的连接或创建一个连接，然后在设备上激活它。如果已经存在配置文件，可以直接使用nmcli connection up <配置文件名>。private表示该连接是否对其他用户可见。hidden用于连接隐藏ssid的wifi。<br><br>wifi hotspot [ifname IFNAME] [con-name NAME] [ssid SSID] [ band { a \| bg } ] [channel channel] [password password]<br><br>创建一个热点，con-name是配置名。<br><br>wifi rescan [ifname IFNAME] [ssid SSID...]<br><br>重新扫描wifi接入点，可以指定多个SSID。对于隐藏ssid的网络，务必要指定ssid。命令不显示接入点列表。显示接入点列表请使用nmcli device wifi list命令

|   |   |
|---|---|
|nmcli dev wifi list|列出可用的wifi接入点, list可以省略|
|nmcli  device wifi connect mySSID password '12345678'|连接热点mySSID, 连接成功后，就会自动生成配置文件，以后要再连接，可以使用nmcli connectio up mySSID命令了|
|nmcli device wifi hotspot con-name ap001 ifname wlp3s0 ssid myAP001 password 12345678|创建热点。以后如果要使用，可以直接nmcli connection up ap001|

### 例子



- wifi 设备扫描
```shell
Nmcli device wifi 
```

- 连接 wifi
```shell
Nmcli device wifi connect ssid password wifipassword 

# 或

nmcli connection add type wifi con-name wifi_name  ifname interface ssid  ssid 
nmcli connection modify wifi_name wifi-sec.key-mgmt wpa_psk wifi-sec.psk wifi_password 
```

- 开启热点
```shell
nmcli device wifi hotspot ifname wlan0 con-name MyHostspot ssid MyHostspotSSID password 12345678
```

- 查看当前 wifi 连接信息
```shell
nmcli device wifi show 
```

- 激活连接
```shell
nmcli connection up wifi_name 
```

- 账号密码连接
```shell
nmcli connection add type wifi con-name NAME ifname wlan0 ssid SSID -- wifo-sec.key-mgmt wpa-eap 802-1x.eap ttls 802-1x.phase2-auth mschapv2 802-1x.identity USERNAME

<!-- ask: 提示输入密码 -->
nmcli --ask connection up NAME
```

- PEAP 连接
```shell
# nmcli con add type wifi ifname wlan0 con-name CONNECTION_NAME ssid SSID
# nmcli con edit id CONNECTION_NAME
nmcli> set ipv4.method auto
nmcli> set 802-1x.eap peap
nmcli> set 802-1x.phase2-auth mschapv2
nmcli> set 802-1x.identity USERNAME
nmcli> save
nmcli> activate

<!--nmcli> set 802-1x.password PASSWORD-->
<!--nmcli> set 802-1x.anonymous-identity ANONYMOUS-IDENTITY-->
<!--nmcli> set wifi-sec.key-mgmt wpa-eap-->
```


- 删除连接配置
```shell
nmcli connection del wifi_name 
```

- 停止连接
```shell
nmcli connection down wifi_name 
```

|                |                                                                                                                                                             |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 接口的详细信息 | nmcli device show interface-name                                                                                                                            |
| 停用连接       | nmcli connection down connection-name                                                                                                                       |
| 激活连接       | nmcli connection up connection-name                                                                                                                         |
| 断开设备连接   | nmcli device disconnect interface-name                                                                                                                      |
| 启用设备连接   | nmcli device connect interface-name                                                                                                                         |
| 新建拨号连接   | nmcli con edit type pppoe con-name “name”<br><br>nmcli>set pppoe.username xxxx<br><br>nmcli>set pppoe.password xxxx<br><br>nmcli>save<br><br>nmcli>activate |
|查看保存的连接配置|nmcli connection show|               |                                                                                                                                                             |
