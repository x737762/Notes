# 挂在 smba 共享目录

- 安装 Samba 客户端软件包

```bash
sudo apt install cifs-utils
```

- 创建挂载点

```bash
sudo mkdir /mnt/samba
```

- 挂载 Samba 共享目录

```bash
sudo mount -t cifs //workgroup/server_name/share_name /mnt/samba
# or
sudo mount -t cifs //192.168.31.31/share /mnt/samba
```

- 其它参数
  - `-o username=username`：指定用于连接到 Samba 服务器的用户名。
  - `-o password=password`：指定用于连接到 Samba 服务器的密码。
  - `-o domain=domain`：指定 Samba 服务器所在的域。
  - `-o workgroup=workgroup`：指定 Samba 服务器所在的工作组。

```bash
sudo mount -t cifs //WORKGROUP/server1/share /mnt/samba -o username=user1,password=password1
# or
sudo mount -t cifs //192.168.31.31/share /mnt/samba -o username=user1,password=password1
```

- 开机自动挂载

编辑 `/etc/fstab` 文件，添加下面内容：

```bash
//192.168.31.31/share /mnt/samba cifs username=user1,password=password1,domain=WORKGROUP 0 0
```

`0 0`：指定文件系统的转储次数和同步次数。
