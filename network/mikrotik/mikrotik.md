Mikrotikにかんするメモ

# 設定回り
Cisco慣れしている人はかなり勝手が違うのでつらい(体験談)

## 基本
rootからコマンドを順に追っていく形。Ciscoのような`show int`ではなく`int ether print`となるように、ある意味Ciscoと逆にshowが最後に来るイメージ。

L2で動かすときはbridgeというグループ(?)に各interfaceをつなげるような形になる模様。

Serial設定で普通じゃないのは115200とRTS/CTSくらい
```
Default settings of the router's serial port are 
115200 bits/s (for x86 default is 9600 bits/s),
8 data bits,
1 stop bit,
no parity,
hardware (RTS/CTS) flow control. 
```
ソースは[公式](https://help.mikrotik.com/docs/display/ROS/Serial+Console)


[えらいひと](https://www.rb-ug.jp/blog/2674.html)が言ってる通りRouterOSはL3動作もできなくはないけどそういうもんじゃないのでL2はちゃんとL2で使う、L3で使いたかったらルーターを買う

### findを使った指定
Ciscoでポートにaccess vlanふるときはこんな感じだけど
```
int Gi 0/1
switchport mode access
switchport access vlan 1000
```

mikrotikの場合はfindでいちいち調べないといけない。そこが少し面倒
```
/interface bridge port set bridge=xxxx [find interface=ether1] pvid=1000
```
ちなみに下記と同じような指定になる。普通ならether1は#0のため
```
/interface bridge port set bridge=xxxx 0 pvid=1000
```

### 個人的によく使うコマンド
- いわゆる`show mac address-table`。bridgeでしか動かしていない前提。
```
interface bridge host print
```
- Link Speedを確認する時
```
interface ethernet monitor ether1
```

## 初期設定
VLAN 1000 / GW 192.168.0.1/24なNWに追加する場合。

結局Ciscoでいう`int vlan`の設定がよくわからなかったのでbridgeそのものにIPを振っている。あとはNTPとかDNSとか基本的な設定

```
/interface bridge set name=bridge pvid=1000 vlan-filtering=yes
/ip address set 0 address=192.168.0.2/24 interface=bridge

/system clock set time-zone-autodetect=no time-zone-name=Asia/Tokyo
/system ntp client set enabled=yes primary-ntp=192.168.0.1
/ip route add distance=1 gateway=192.168.0.1
/ip dns set servers=192.168.0.1
/system package update set channel=long-term
```

