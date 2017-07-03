# Ubuntu上网总结
## (A)在init3 状态下联网：
<a>在recovery模式启动ubuntu会有联网选项，点完就能连了，应该是自动按此前保存的配置联网的。</a>
<a>使用终端命令行 iwconfig命令 连接wifi：</a>
* ifconfig查到本机无线网卡名称（一般是wlan0, 我的雷蛇是wlp59s0）
* sudo ip link set wlan0 up确认启动这个网卡接口
* 搜索无线网 iwlist wlan0 scan
* 记下essid 它使用的是哪个安全加密的（如：WEP、WPA/WPA2）
###### 如果是WEP协议，简单 
* 连接无密码的无线网 iwconfig wlan0 essid ChinaNet　其中ChinaNet是搜索到的无线网essid
* 连接有密码的无线网 iwconfig wlan0 essid ChinaNet key xxxx　其中xxxx是密码
###### 如果网络使用的是WPA或者WPA2协议，则需要使用一个叫做wpasupplicant的工具，通过如下命令可以自动安装：
* sudo apt install wpasupplicant
* 然后vi一个配置文件[/etc/wpasupplicant/wpa_supplicant.conf],可以用
> wpa_passphrase ESSID PWD > xxx.conf  
  {wpa_supplicant -B -i wlan0 -Dwext -c ./xxx.conf  
  iwconfig wlan0  
  dhclient wlan0}
* 或者手动改成如下内容：
> ctrl_interface=/var/run/wpa_supplicant  
  ap_scan=1  
  network={  
        ssid="[your SSID name]"  
        psk="[your WiFi password]"  
        priority=1  
  }
* sudo wpa_supplicant -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
* 最后，无论是连到开放的网络还是加密的安全网络，您都得获取 IP 地址。使用命令dhclient wlan0　或 dhcpcd wlan0
## （B）比较完整的网络配置说明 <http://blog.csdn.net/judwenwen2009/article/details/17026389>
* 无法上网，右上角显示感叹号，先检查/etc/network/interfaces文件，内容居然只有：
> auto lo  
  iface lo inet loopback
* eth0和wlan0的配置都不见了！增加如下配置：
> auto eth0  
  ifcace eth0 inet static //学校网络要求是配置静态IP，如果是动态获取IP下边的几行就不需要，改成iface eth0 inet dhcp就可以了  
  address xx  
  gateway xx  
  netmask xx  
  auto wlan0  
  iface wlan0 inet dhcp`
* 同时，有线网络的dns也不见了，在/etc/resolv.conf中添加 namesever XX {一般是google的8.8.8.8 dns}
* 重启网络/etc/init.d/networking restart
