---
layout:     post
title:      "🍎M1 mac环境编译TeXStudio"
subtitle:   "💵该换M2 Pro了"
date:       2023-05-31 23:59:00
author:     "Shicong Liu"
header-img: "img/home-dark.jpg"
catalog: true
mathjax: true
comment: true
hide-in-nav: true
tags:
    - 日常
    - feed
---



已经有两个月没有发布任何信息了。这段时间里发生了很多事情，许多计划在纠结中一地鸡毛，直到今天许多事情都没能真正尘埃落定。距离正式毕业还有三四天的时间，就趁现在写几篇轻松些的内容好了。



在苹果发布M1系列芯片后，许多软件需要针对Apple Silicon的arm64架构重新编译。经过几年的迭代，大部分仍在维护的软件都实现了原生支持arm64架构，不需要进行Rosetta 2转译，从而一定程度上避免了性能损失。事到如今我的常用软件里只有TexStudio和MATLAB仍然是x86_64平台编译的，通过转译实现跨平台支持。MATLAB早些时候提到原生支持Apple Silicon的版本正在开发，然而除了一年前开放测试的阉割版2022b[^1]以外，完整版的支持始终杳无音信。

相比闭源商业软件MATLAB，TeXStudio是开源软件[^2]，允许我们在本地从源码构建。为了治愈强迫症，我花了一下午的时间实现了~~踩雷~~本地编译。

准备工作，配置依赖

```bash
brew reinstall $(brew deps poppler)
brew install vips
brew install freetype
brew install clang-format
brew install qt
```

实际上`$(brew deps poppler)`就包含了freetype，但由于编译器寻包策略，我本地机器上的MATLAB Runtime顶替了原本macOS的arm64版依赖，导致了后续许多错误。此外此处的`qt`默认是`Qt@6`，在macOS上brew安装的`Qt@5`本身不主动承担作为依赖项的职能，texstudio本身也可以从qt6构建。

但是我们并不能从brew上直接安装合适的poppler。基于qt6构建的poppler在brew上构建失败了，brew也并没有计划设置合适的formula支持，因此需要从官方源构建poppler。为节约时间，从官方页面[^3]下载最新版本(此处使用的是五月初发布的[`poppler-23.05.0.tar.xz`](https://poppler.freedesktop.org/poppler-23.05.0.tar.xz))。

解压后进入根目录，修改`CMakeLists.txt`第74行

```cmake
option(ENABLE_QT5 "Compile poppler qt5 wrapper." ON)
```

为

```cmake
option(ENABLE_QT5 "Compile poppler qt5 wrapper." OFF)
```

取消对Qt5的编译过程。

此后按顺序执行

```bash
cd build
cmake ..
cmake --build . --parallel
sudo cmake --build . -t install
```

也可以直接参考根目录下的`INSTALL`文件操作，此处不再赘述。

构建并安装了`poppler-qt6`之后，下载texstudio的[最新源码](https://github.com/texstudio-org/texstudio/releases/tag/4.5.2)

解压后仍然修改`CMakeLists.txt`第47行

```cmake
find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets LinguistTools REQUIRED)
```

为

```cmake
find_package(QT NAMES Qt6 COMPONENTS Widgets LinguistTools REQUIRED)
```

直接取消对qt5的搜索。

> 不同版本的CMakeLists可能会有不同，请根据实际情况调整

完成后执行

```bash
cd build
cmake ..
cmake --build . --parallel
```

编译结束后会在`build`下生成`texstudio.app`，将其复制到Application中即可。

---

本地构建过程中遇到了许多问题，包括在寻包过程中意外使用`x86_64`的MATLAB Runtime，Qt5的不佳支持无法有效搜索到PDF渲染器poppler等。至此除了商业软件MATLAB以外，所有常用软件已经全部迁移到arm64平台。



---

[^1]:[MATLAB Apple Silicon 2022b测试](https://ww2.mathworks.cn/support/apple-silicon-r2022b-beta.html)
[^2]:[TexStudio GitHub](https://github.com/texstudio-org/texstudio)
[^3]:[Poppler官方发布页](https://poppler.freedesktop.org/)
