## 大作业
在Ubuntu从github克隆的时候，下载速度只有不到10KB/s。于是去网上搜索解决方法。
### 方法一：
```
sudo vim /etc/ssh/ssh_config
 
将GSSAPIAuthentication yes
改为
GSSAPIAuthentication no
```
> 尝试发现网速并没有变化，依然是几KB/s。
> 1111
> 2222
