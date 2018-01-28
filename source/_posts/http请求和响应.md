---
title: flag
---


### 一、user-agent

### 看一下Chrome的User-Agent

### 这里可以看出来，该Chrome是MAC的发布版，采用了兼容了Mozilla，Safari，内核兼容AppleWebkit和Gecko
* User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
* 看一下火狐的User-Agent
可以看出来，该Firefox为Windows平台的发布版，内核为Firefox的自家内核Gecko
* User-Agent:Mozilla/5.0 (Windows NT 6.1; WOW64; rv:30.0) Gecko/20100101 
* Firefox/30.0
* 看一下Mac上的Safari
* 看出来，兼容了Mozilla，为Mac系统的发布版，采用自家的Webkit内核(Apple)
* User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) 
* 
* AppleWebKit/537.75.14 (KHTML, like Gecko) Version/7.0.3 Safari/537.75.14

* 最后是IE的User-Agent

  看出来，兼容了Mozilla，采用兼容模式的IE10，采用自家的Trident内核
* User-Agent:Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0)
 * 模拟移动设备
* 我们这里可以模拟一个移动设备来查看一下，显示效果，模拟设备，Galaxy Note II ，通过该设备向服务端发送请求

* 请求标头，
* User-Agent:Mozilla/5.0 (Linux; U; Android 4.1; en-us; GT-N7100 Build/JRO03C) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30

二、host  主机域名，或ip地址


三、accept

四、refer

五、gzip

六、ttransfer-encoding



