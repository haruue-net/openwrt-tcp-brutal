在 OpenWrt 上编译安装 tcp-brutal
==============

## 前置条件

1. 配置好 OpenWrt 编译环境。
   通常来说一台 Debian 12 的 Linux 机器就可以， 请参考
   [OpenWrt 官方指南](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem)
   来了解要装哪些包：

2. 准备适用于你当前使用的 OpenWrt 版本的 SDK。
   + 如果你的 OpenWrt 是从官网下载的稳定版，
     通常可以直接在下载页面找到 `openwrt-sdk-<版本号>-*.tar.xz` 的文件，
     版本号必须和你正在使用的完全相同。
   + 如果你的 OpenWrt 是从官网下载的 snapshot 版，
     并且下载的时候有注意留存上述的 SDK 文件， 那么也能直接使用。
     如果没有留存， 那很不幸通常没有现成的 SDK 可用， 建议重装成稳定版。
   + 如果你的 OpenWrt 是从第三方下载的「定制版」， 请尝试从下载页面获取，
     或者直接联系维护者把本仓库编译进定制版本中。（你自己编译的也一样）

为了方便， 下面以 OpenWrt 官方
[23.05.3 x86-64](https://archive.openwrt.org/releases/23.05.3/targets/x86/64/)
版本的 SDK 为例进行介绍。


## 编译步骤

1. 解压 SDK 文件。

   ```bash
   tar xf openwrt-sdk-23.05.3-x86-64_gcc-12.3.0_musl.Linux-x86_64.tar.xz
   cd openwrt-sdk-23.05.3-x86-64_gcc-12.3.0_musl.Linux-x86_64
   ```

2. 配置 feeds， 由于 tcp-brutal 只依赖内核， 通过 SDK 编译不需要任何别的 feeds。

   ```bash
   echo "src-git tcp_brutal https://github.com/haruue-net/openwrt-tcp-brutal;master" > feeds.conf.default
   ./scripts/feeds update -a && ./scripts/feeds install -a
   ```

3. 进行编译

   ```bash
   make defconfig
   make package/feeds/tcp_brutal/tcp-brutal/compile
   ```

4. 寻找编译好的 ipk 文件， 通常是下面这种位置，
   路径会随架构、 内核版本以及 tcp-brutal 自身的版本发生改变。

   ```
   <SDK解压目录>/bin/targets/x86/64/packages/kmod-brutal_5.15.150+1.0.3-1_x86_64.ipk
   ```

将通过上述步骤获得的 `kmod-brutal*.ipk` 文件复制到 OpenWrt 上使用 `opkg install` 安装，
或者通过「系统」-「软件包」-「上传软件包…」进行安装即可使用。

每次系统升级之后， 都需要重新进行编译和安装。


## 将 tcp-brutal 整合进定制版 OpenWrt

假设你已经知道怎么编译 OpenWrt。

在配置 feeds 时， 往 feeds.conf.default 中追加下面这行， 而不是替换整个文件。

```
src-git tcp_brutal https://github.com/haruue-net/openwrt-tcp-brutal;master
```

在执行 `menu config` 时， 依次进入「Kernel modules」-「Network Support」，
选中 `kmod-brutal` 并改成 `<*>`。

