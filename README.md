# 在Archlinux及衍生发行版上运行TIM

Deepin打包的TIM容器移植到Archlinux，包含定制的注册表配置，TIM安装包替换为官方最新

构建状态: [![travis-ci](https://travis-ci.org/countstarlight/deepin-wine-tim-arch.svg?branch=master)](https://travis-ci.org/countstarlight/deepin-wine-tim-arch)

感谢:

* [Wuhan Deepin Technology Co.,Ltd.](http://www.deepin.org/)

* [@wszqkzqk](https://github.com/wszqkzqk) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/wszqkzqk/wszqkzqk-deepin-wine-tim-arch)

* [@ssfdust](https://github.com/ssfdust) 的 [wszqkzqk-deepin-wine-tim-arch](https://github.com/ssfdust/wszqkzqk-deepin-wine-tim-arch)

存在如下问题：
  1. 不能视频通话
  2. 不能记住密码
  3. 网络连接状态改变后不能重连: [issue19](https://github.com/countstarlight/deepin-wine-tim-arch/issues/19)

## 安装

`deepin-wine-tim`依赖`Multilib`仓库中的`wine`，`wine_gecko`和`wine-mono`，Archlinux默认没有开启`Multilib`仓库，需要编辑`/etc/pacman.conf`，取消对应行前面的注释([Archlinux wiki](https://wiki.archlinux.org/index.php/Official_repositories#multilib)):

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

* 1.已添加到AUR [deepin-wine-tim](https://aur.archlinux.org/packages/deepin-wine-tim/)，可使用 yay 安装安装:
```shell
 yay -S deepin-wine-tim
```

* 2.手动安装

```shell
 git clone https://github.com/countstarlight/deepin-wine-tim-arch.git

 cd deepin-wine-tim-arch
  
 makepkg -si
```

* 运行开始菜单中创建的TIM，点击立即安装
* 安装完可直接启动，以后启动时无需安装
* 默认使用文泉驿微米黑(`wqy-microhei`)字体，要使用其他字体，如 微软雅黑或者微软宋体放进`～/.deepinwine/Deepin-TIM/drive_c/windows/Fonts`中。
## 常见问题
* 1.在 2k/4k 屏幕下字体和图标都非常小, 参见[issue1](https://github.com/countstarlight/deepin-wine-tim-arch/issues/1)
* 2.使用全局截图快捷键和解决Gnome上窗口化问题，参见[issue2](https://github.com/countstarlight/deepin-wine-tim-arch/issues/2)
## 更新日志

* 2019-02-05 TIM-2.3.1_3
* 2018-02-23 TIM-2.1.5
* 2017-12-23 TIM-2.1.0
* 2017-11-28 修复音频功能（麦克风录音和播放语音消息）
* 2017-11-21 TIM-2.0.0
