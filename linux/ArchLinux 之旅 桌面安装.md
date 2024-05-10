## 配置用户

- 创建普通用户

```bash
useradd -m -G wheel shi # shi: 用户名
passwd shi # 设置普通用户密码
```

- 让普通用户能使用 sudo 执行管理员命令

```bash
pacman -S sudo
vim /etc/sudoers
# 取消 %wheel ALL=(ALL) ALL 前面的注释
```

## 安装中文字体设置中文

### 使用 Pacman 安装

```bash
sudo pacman -S <package_name>
```

`<package_name>`：字体名称

```bash
sudo pacman -S wqy-zenhei
```

### 手动安装

- 安装字体管理工具

```bash
sudo pacman -S fontconfig
```

- 将字体文件复制到 `/usr/share/fonts` 或 `~/.local/share/fonts` 目录下。
  - `/usr/share/fonts`：目录是系统范围的字体目录。
  - `~/.local/share/fonts`：目录是用户特定的字体目录。
- 更新字体缓存 `fc-cache -fv`。

### 设置系统语言

**注意：** 不推荐在 `/etc/locale.conf` 里把全局的 LANG locale 设置成中文 `LANG=zh_CN.UTF-8`，因为 TTY 下没有 CJK 字体，这样设置会导致 TTY 中显示豆腐块（除非你使用的内核打了 cjktty 补丁能绘制中文字体，比如 linux-lily）。

<br>

**每个用户单独的 locale 可以在` ~/.bashrc`、`~/.xinitrc` 或 `~/.xprofile` 中设置，即添加下面两行到文件的最开头（如果不确定使用哪个文件，都添加）：**

```bash
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
```

- `.bashrc`：每次使用终端时会应用此处的设置。
- `.xinitrc`：每次使用 startx 或 SLiM 来启动 X 窗口系统时会应用此处的设置。
- `.xprofile`：每次使用 GDM 等显示管理器时会应用此处的设置。
