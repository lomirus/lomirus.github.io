---
title: "WSL 2 入门使用与踩坑记录"
date: 2021-05-28T00:00:00+08:00
lastmod: 2021-06-02T18:25:17+08:00
image: "cover.png"
description: "WSL 2 + Windows Terminal + VSCode"
categories: ["Code"]
tags: ["WSL2", "Windows Terminal", "VSCode", "Linux"]
slug: "wsl2"
---

## 起因

最近我用 rust 写了个 shogi game. 考虑到性能优化及用户体验，控制台采用 raw mode 进行渲染。结果发现兼容性不是很好，在各种 shell/terminal 上表现不尽相同。而在字体渲染上，有一说一，除了 Windows Terminal 其余的都是辣鸡。

我（之前）平常最常用的 shell 是 Git Bash，但是正如前文所言，界面着实拉跨；而使用 Windows Terminal 打开 Git Bash 时，按退格键时又会出现闪烁现象。最终忍无可忍，于是把这些 bash/shell 全都拉出来批判一番：

- CMD: 费拉不堪
- Powershell：费拉不堪
- Gitbash: 费拉不堪

由于 Windows 上的 shell 人均费拉不堪，所以问了一位后端朋(dà)友(lǎo)有没有什么推荐：

![2FXjHS.jpg](https://z3.ax1x.com/2021/05/28/2FXjHS.jpg)

结果发现人家对 shell 根本就不像我这么挑剔😂，不过也可能确实是受方向影响。因为毕竟后端首先在 Windows 方面上有成熟的 Goland IDE，确实一般情况下用不到控制台，用到终端的话一般就都是在服务器上了，而服务器则一般都是用的 Linux，所以通常没啥顾虑。而我等苦逼前端菜鸡还是要经常和各种 CLI 打交道的，所以最好还是去找一款优秀的终端软件。

然后最终选定了 WSL 2 + Windows Terminal + VSCode.


## 安装

如果 Windows 10 OS Version 大于 20262，可通过 `wsl --install` 安装。

虽然我有加入 Windows Insider 计划，不过系统还是用的 Stable，所以没有采取上述简化方式进行安装，而是采用的以下内容。

首先，以管理员权限打开 Powershell 并输入以下命令：

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

输入以下命令启用“虚拟机平台”功能，然后重启：
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

然后下载[适用于 x64 计算机的 WSL2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)并安装，以将 WSL 1 更新至 WSL 2。

安装好后设置 WSL 2 为默认版本：

```powershell
wsl --set-default-version 2
```

打开[Microsoft Store](https://aka.ms/wslstore)，随便选择一种发行版并安装。这里我选的是大众喜闻乐见的 Ubuntu。安装完后启动，执行一些配置就 OK 了。

最后可以查看一下已安装的 WSL 发行版的版本信息：

```powershell
wsl --list --verbose
```

顺便说一下，安装新的发行版的操作并不会使你旧的发行版被替换掉。如果你之前安装过 WSL 1 的 Linux 发行版，而又不想将其保留，也可以将其删除掉。比如，如果我想将之前的安装的 WSL 1 Debian 删除掉，可执行以下命令：

```powershell
wsl --unregister debian
```

其余操作可以使用 `wsl -h` 或查看[官方文档](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)。

## 网络代理

由于 WSL 2 是通过虚拟机的方式实现，所以不能像 WSL 1 时期那样直接使用 `127.0.0.1` 修改 WSL 内的网络代理。此时正确的处理方式应该是向 `~/.bashrc` 添加如下内容：

```sh
hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
port=7890 # 此处端口可根据个人情况进行更改

PROXY_HTTP="http://${hostip}:${port}"

export http_proxy="${PROXY_HTTP}"
export HTTP_PROXY="${PROXY_HTTP}"

export https_proxy="${PROXY_HTTP}"
export HTTPS_proxy="${PROXY_HTTP}"
```

参考文章：[WSL2 中访问宿主机 Windows 的代理](https://zinglix.xyz/2020/04/18/wsl2-proxy/)

如果遇到代理访问超时的问题，且使用的是 Clash for Windows，请尝试开启 Clash for Windows 中主界面中的 Allow LAN 选项。

## 环境变量

在 WSL 2 中默认可获取到宿主机中的环境变量，这可能会导致某些错误，比如获取到一些没有意义的环境变量，导致环境混乱。

若要禁止在 WSL 2 中访问 Windows 环境变量，可以先创建`/etc/wsl.conf`：

```bash
touch /etc/wsl.conf
```

然后修改其内容为：

```
[interop]
appendWindowsPath = false
```

## 常用软件的安装

接下来安装几个我常用的软件包，这部分没什么技术含量，仅供以后查阅方面使用。

首先更新软件源：

```bash
sudo apt update
```

然后安装：

```sh
# python
sudo apt install python3
ln -s /usr/bin/python3 /usr/bin/python

## node/npm
sudo apt install nodejs
sudo apt install npm

## yarn
sudo npm install -g yarn

## deno
sudo apt install unzip
curl -fsSL https://deno.land/x/install/install.sh | sh

## rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# 建议选择 `default toolchain` 为 `nightly`
```

## 其他坑（先占位）

由于 WSL 2 用到了虚拟化平台功能，可能会导致其他虚拟机（如 VirtualBox）无法运行。目前我还没解决这个问题，啥时候用空解决了再更。

更新：解决方案：卸载 VirtualBox, 安装VMWare.