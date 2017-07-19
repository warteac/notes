### VNC installation and configuration

#### vnc
- stands for virtual network computing, 属于远程控制类软件，优点是支持跨操纵系统的远程图形化控制。
- 可以服务器配置多个桌面供远程访问，需要在/etc/sysconfig/vncservers中配置，每个桌面对应一个端口
- 可以使用默认端口5900+桌面号，或者自定义端口号，需要在/usr/bin/vncserver中修改222行和419行。
- 参考资料：
[http://www.centoscn.com/image-text/install/2015/0211/4687.html](http://www.centoscn.com/image-text/install/2015/0211/4687.html)

- desktop and cnv server installation:

```

	$sudo yum groupinstall "Desktop"

	$sudo yum groupinstall "X Window System"

	$sudo yum install tigervnc-server
```

- configure the gnome

```

	$vi .vnc/xstartup
```

making the last three lines like this: 
![](http://i.imgur.com/uDa5Z6t.png)

- start up vnc server

```

    $vncserver

	$vncserver : 1

```


- reset password

```
	$vncpasswd
```

- restart vnc servie


```

	$service vncserver restart

```

- configure the desktop

```

	$vi /etc/sysconfig/vncservers
	
```

e.g.
![](http://i.imgur.com/rW4albt.png)


- configure vnc service port

```

	$which vncserver

	$/usr/bin/vncserver
```

修改222和419行的端口号

```

	222:$vncPort = 8001 + $displayNumber;


	419: if (!bind(S, pack('S n x12', $AF_INET, 8001 + $n))) {

```

保存文件，重启服务


- 查看端口情况：

```
	ps -ef|grep vnc
```

> 某桌面占用端口号为：配置端口号+桌面号，比如配置为8001，桌面号为1，则实际的端口号为8002.

- 关闭服务(例如 2号桌面）

```

	vncserver -kill ：2

```


