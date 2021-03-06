在Archlinux及衍生发行版上运行TIM
========

<p align="center">
  <a href="https://github.com/countstarlight/deepin-wine-tim-arch/actions">
    <img src="https://img.shields.io/github/workflow/status/countstarlight/deepin-wine-tim-arch/CI/action?logo=github&style=flat-square" alt="Build Status">
  </a>
  <a href="https://office.qq.com/download.html">
    <img src="https://img.shields.io/badge/TIM-3.3.8.22043-blue?style=flat-square" alt="TIM Version">
  </a>
  <a href="https://aur.archlinux.org/packages/deepin-wine-tim/">
    <img src="https://img.shields.io/aur/version/deepin-wine-tim?label=AUR&logo=arch-linux&style=flat-square" alt="AUR Version">
  </a>
  <a href="https://github.com/countstarlight/deepin-wine-tim-arch/releases">
    <img src="https://img.shields.io/github/downloads/countstarlight/deepin-wine-tim-arch/total?logo=github&style=flat-square" alt="GitHub Release">
  </a>
  <a href="https://github.com/countstarlight/deepin-wine-tim-arch/issues">
    <img src="https://img.shields.io/github/issues/countstarlight/deepin-wine-tim-arch?logo=github&style=flat-square" alt="GitHub Issues">
  </a>
</p>

Deepin 打包的 QQ 容器(`com.qq.im.deepin`)移植到 Archlinux，QQ 环境修改为 TIM，不依赖 `deepin-wine`，包含定制的运行脚本，TIM 安装包为官方最新

<!-- TOC -->

- [安装](#安装)
    - [从AUR安装](#从aur安装)
    - [用安装包安装](#用安装包安装)
    - [本地打包安装](#本地打包安装)
- [设置](#设置)
- [兼容性记录](#兼容性记录)
- [切换到 `deepin-wine`](#切换到-deepin-wine)
    - [自动切换(推荐)](#自动切换推荐)
    - [从 `deepin-wine 2.x` 迁移](#从-deepin-wine-2x-迁移)
- [卸载](#卸载)
- [常见问题及解决](#常见问题及解决)
    - [不能记住密码](#不能记住密码)
    - [网络连接状态改变后不能重连](#网络连接状态改变后不能重连)
    - [高分辨率屏幕支持](#高分辨率屏幕支持)
    - [GNOME 桌面上的悬浮窗口问题](#gnome-桌面上的悬浮窗口问题)
    - [使用其他字体](#使用其他字体)
- [感谢](#感谢)
- [更新日志](#更新日志)

<!-- /TOC -->

## 安装

`deepin-wine-tim` 依赖`Multilib`仓库中的 `wine`，`wine-gecko` 和 `wine-mono`，Archlinux 默认没有开启 `Multilib`仓库，需要编辑`/etc/pacman.conf`，取消对应行前面的注释([Archlinux wiki](https://wiki.archlinux.org/index.php/Official_repositories#multilib)):

```diff
# If you want to run 32 bit applications on your x86_64 system,
# enable the multilib repositories as required here.

#[multilib-testing]
#Include = /etc/pacman.d/mirrorlist

-#[multilib]
-#Include = /etc/pacman.d/mirrorlist
+[multilib]
+Include = /etc/pacman.d/mirrorlist
```

:warning: **注意：由于新版TIM可能需要 `wine` 还没有实现的一些win api，这会导致一些功能不可用，安装前先根据[兼容性记录](#兼容性记录)选择一个合适的版本**

:bulb: 以下三种安装方式效果相同，选择一种即可

### 从AUR安装

已添加到 AUR [deepin-wine-tim](https://aur.archlinux.org/packages/deepin-wine-tim/)，可使用 `yay` 或 `yaourt` 安装：

```shell
yay -S deepin-wine-tim
```

### 用安装包安装

> 由 [GitHub Action](https://github.com/countstarlight/deepin-wine-tim-arch/actions) 在 Docker 容器 [countstarlight/makepkg](https://hub.docker.com/r/countstarlight/makepkg) 中自动构建的 ArchLinux 安装包

在 [GitHub Release](https://github.com/countstarlight/deepin-wine-tim-arch/releases) 页面下载后缀为 `.pkg.tar.xz` 或 `.pkg.tar.zst` 的安装包，使用`pacman`安装：

```bash
sudo pacman -U #下载的包名
```

`.md5` 文件用于校验包完整性：

```bash
md5sum -c *.md5
```

### 本地打包安装

```shell
 git clone https://github.com/countstarlight/deepin-wine-tim-arch.git

 cd deepin-wine-tim-arch
  
 makepkg -si
```

用上述三种安装方式之一安装完成后，运行应用菜单中创建的 TIM 快捷方式，首次运行会用 TIM 的安装包进行安装

:warning: **注意：安装 TIM 时不建议修改安装路径，如果修改默认路径，要对应修改 `deepin-wine-tim` 的启动脚本(`/opt/apps/com.qq.office.deepin/files/run.sh`)：**

```bash
EXEC_PATH="c:/Program Files/Tencent/TIM/Bin/TIM.exe"
```
改为修改后的安装路径，否则只有安装后第一次能够运行

:bulb: **NOTE: 前几次运行时可能会提示 "qq安全组件异常"，等一会再运行或重启一下系统**

## 设置

dpi，目录映射等可以在 `winecfg` 进行设置，打开 `winecfg` 的命令为：

```bash
/opt/apps/com.qq.office.deepin/files/run.sh winecfg
```

## 兼容性记录

|     TIM     | wine |   兼容性   |             备注             | deepin-wine | 兼容性 | 备注 |
| :---------: | :--: | :--------: | :--------------------------: | :---------: | :----: | :--: |
| 3.3.8.22043 | 6.12 |            |                              |  5.0.16-1   |  支持  |      |
| 3.3.5.22018 | 6.8  |            |                              |  5.0.16-1   |  支持  |      |
| 3.3.0.22020 | 6.1  |    部分    | 部分字体显示为方框且性能较差 |  5.0.16-1   |  支持  |      |
| 3.2.0.21856 | 5.18 | **不支持** |           无法启动           |  2.18_24-3  |  支持  |      |
| 3.1.0.21789 | 5.16 |    支持    |                              |  2.18_24-3  |  支持  |      |

## 切换到 `deepin-wine`

> 根据 [deepin-wine-wechat-arch#15](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/15#issuecomment-515455845)，[deepin-wine-wechat-arch#27](https://github.com/countstarlight/deepin-wine-wechat-arch/issues/27)，由 [@feileb](https://github.com/feileb), [@violetbobo](https://github.com/violetbobo), [@HE7086](https://github.com/HE7086)提供的方法

`deepin-wine-tim` 默认使用官方仓库中的 `wine`，原版 `wine` 在 [DDE(Deepin Desktop Environment)](https://www.deepin.org/dde/) 上，有托盘图标无法响应鼠标事件([deepin-wine-tim-arch#21](https://github.com/countstarlight/deepin-wine-tim-arch/issues/21))的问题，且原版 `wine` 尚不能实现保存登录密码等功能，可以选择切换到 `deepin-wine`。

:warning: **注意：切换前先确保 `deepin-wine` 支持**

### 自动切换(推荐)

```bash
/opt/apps/com.qq.office.deepin/files/run.sh -d
```

这会安装需要的依赖，移除已安装的 TIM 目录并回退对注册表文件的修改

> 从 `v3.3.0.22020-1` 开始，该命令会切换到 AUR 仓库：[deepin-wine5](https://aur.archlinux.org/packages/deepin-wine5)


如果想切换回原版 `wine` 并卸载为 `deepin-wine` 自动安装的依赖：

```bash
rm $HOME/.deepinwine/Deepin-TIM/deepin
sudo pacman -Rns deepin-wine5
```

### 从 `deepin-wine 2.x` 迁移

若之前使用的是 `deepin-wine 2.x`，更新到 `deepin-wine-tim v3.3.0.22020-1` 及之后的版本会自动切换回 `wine`，运行命令：

```bash
/opt/apps/com.qq.office.deepin/files/run.sh -d
```

就会自动安装并切换到 `deepin-wine5`

若此时没有其他应用在使用旧版 `deepin-wine`，就可以放心的卸载旧版 `deepin-wine` 及其依赖：

```bash
sudo pacman -S lib32-freetype2 #用原版替换lib32-freetype2-infinality-ultimate
sudo pacman -Rns deepin-wine xsettingsd
```

## 卸载

无论用何种方式安装，卸载都是：

```bash
sudo pacman -Rns deepin-wine-tim
```

卸载的同时会删除用户目录下的整个 `WINEPREFIX` 环境，路径为：`~/.deepinwine/Deepin-TIM`

TIM在本地保存的数据不会被删除，如保存在用户文档下的数据(默认：`~/Documents/Tencent Files`)

## 常见问题及解决

### 不能记住密码

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 网络连接状态改变后不能重连

参照[切换到 `deepin-wine`](#切换到-deepin-wine) 解决

### 高分辨率屏幕支持

参照[设置](#设置)打开 `winecfg` ，在选项卡 `Graphics` 中修改dpi，如 修改为`192`

### GNOME 桌面上的悬浮窗口问题

> 根据 [deepin-wine-tim-arch#2](https://github.com/countstarlight/deepin-wine-tim-arch/issues/2)，由[EricDracula](https://github.com/EricDracula)提供的方法

安装 GNOME 插件: [TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)

### 使用其他字体

默认使用文泉驿微米黑(`wqy-microhei`)字体，可以使用Windows平台常用字体替代，直接将字体文件或字体链接文件放置到字体文件夹就会生效，不会影响系统字体

字体文件夹在：`$HOME/.deepinwine/Deepin-TIM/drive_c/windows/Fonts`

## 感谢

* [Wuhan Deepin Technology Co.,Ltd.](http://www.deepin.org/)

* [@wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* [@ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

## 更新日志

<details open>
<summary>2021</summary>

* 2021-07-12 TIM-3.3.8.22043
* 2021-05-11 TIM-3.3.5.22018
* 2021-02-11 TIM-3.3.0.22020 com.qq.im.deepin_9.3.2deepin20
* 2021-02-03 TIM-3.3.0.22020 com.qq.im.deepin_9.3.2deepin14

</details>
<details>
<summary>2020</summary>

* 2020-09-30 TIM-3.2.0.21856
* 2020-08-12 TIM-3.1.0.21789
* 2020-04-01 TIM-3.0.0.21315

</details>
<details>
<summary>2019</summary>

* 2019-09-21 TIM-2.3.2.21173
* 2019-03-06 TIM-2.3.2.21158
* 2019-02-05 TIM-2.3.1_3

</details>
<details>
<summary>2018</summary>

* 2018-02-23 TIM-2.1.5

</details>
<details>
<summary>2017</summary>

* 2017-12-23 TIM-2.1.0
* 2017-11-28 修复音频功能（麦克风录音和播放语音消息）
* 2017-11-21 TIM-2.0.0

</details>