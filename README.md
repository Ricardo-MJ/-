# 大作业
## 在Ubuntu从github克隆的时候，下载速度只有不到10KB/s。于是去网上搜索解决方法。
#### 方法一：
```
sudo vim /etc/ssh/ssh_config
 
将GSSAPIAuthentication yes
改为
GSSAPIAuthentication no

```
**尝试发现网速并没有变化，依然是几KB/s。**
<br/>
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
**尝试发现网速变成了300~500KB/s，亲测有效。**
<br/>
<br/>
<br/>
## 在make到97%的时候突然停止，Ubuntu警告：内存不足。
```
由于最初创建虚拟机的时候只分配了40个G的内存给Ubuntu，所以导致内存不够。
于是退出虚拟机，在VMware中重新给Ubuntu分配了200个G的内存，并且进入虚拟机中的磁盘，把新分配的内存全部拉满。
重新make，以为要全部重新make一遍，结果发现是继续从97%开始make。最后make成功。
```
<br/>
<br/>
<br/>
## 连接到SSH
```
使用指令 ssh-keygen -t rsa -C "XXXXX@qq.com" 连接到自己邮箱注册的github，并生成一段密码。
再使用指令 cat ~/.ssh/id_rsa.pub 来获取得到生成的密码。


```
