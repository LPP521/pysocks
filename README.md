

## 介绍

一个类似go什么age什么nt的工具，采用服务器+客户端的结构，但是区别在于是使用了socks5协议

## 使用

将server.py放入服务器中，例如vps，然后运行
python server.py

或者用nohup运行
nohup python server.py &

客户端在本地运行，监听本地端口，将浏览器的socks5代理设置为这个
（在firefox里面可以把network.proxy.socks_remote_dns设置为true，效果更好，否则因为某些解析问题，比如说“你水管”可能上不去）

## 技术

构建了一个从本地到服务器的“加密”数据连接，讲本地的socks5代理请求转发到远程服务器，由远程服务器处理代理请求再传回客户端，客户端再将数据转送给应用程序。
（个人以为socks5的实现是一个仅仅比VPN慢一点，但是在很多情况可以比http proxy方便一点点的工具，甚至快一点，至少没有https可能导致的证书问题。其实goxxxxx不实现socks二区实现http主要应该是因为GAE没办法，而我主要是想用在自己的VPS上的所以没关系）

## 实现

实现了基本的socks5的TCP服务器
并没有实现了真正的"加密"，暂时只是将数据加盐（还加的非常简单，只交换了字节的高4位第四位位置），不过基本上能达到可以“用”的水平，中国人应该都知道这个可以用是什么意思吧

## 没实现的
socks5 的UDP
socks协议的 bind方式，除了FTP以外我真不知道有程序真的会用这个，FTP其实不用passive模式这个也没啥用
可能对部分地址类型支持的不好，暂时支持IPv4，IPv6和domain地址，但是程序调试中检测到了其他类型地址（也有可能只是出错了），所以不知道

