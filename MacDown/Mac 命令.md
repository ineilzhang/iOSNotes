### Mac 命令

#### WIFI 相关

- 查看网络硬件设备
  ```networksetup -listallhardwareports```  
- 关闭WIFI
  ```networksetup -setairportpower en0 off```
- 开启WIFI
  ```networksetup -setairportpower en0 on```
- 扫描附近 WIFI 热点
 ```/System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport scan```
- 加入WIFI
```networksetup -setairportnetwork en0 TPLINK_10000_5G 8888881
```

#### 重启
- 重启并发送 SSH 登陆者信息
```sudo shutdown -r now```
