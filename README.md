## 大作业
### 在Ubuntu从github克隆的时候，下载速度只有不到10KB/s。于是去网上搜索解决方法。
#### 方法一：
```
sudo vim /etc/ssh/ssh_config
 
将GSSAPIAuthentication yes
改为
GSSAPIAuthentication no

```
**尝试发现网速并没有变化，依然是几KB/s。**


#### 方法二：
```
使用指令 
nslook global.ssl.fastly.NET
nslook github.com
来获取域名的IP地址

再使用 sudo gedit /etc/hosts 指令
进入host中
加上如下两行：

XXX.XXX.XX.XXX global-ssl.fastly.Net    
XXX.XXX.XX.XXX github.com       //XXX.XXX.XX.XXX为使用上面的nslook指令得到的IP地址

最后使用指令 sudo /etc/init.d/networking restart 刷新DNS缓存
```
