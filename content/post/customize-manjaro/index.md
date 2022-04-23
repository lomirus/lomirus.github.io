---
title: "Customize the Manjaro with Gnome"
date: 2021-07-20T00:00:00+08:00
lastmod: 2021-08-25T00:00:00+08:00
image: "cover.jpg"
description: "Since I cannot stand the Windows, I embraced the Linux. "
categories: ["Code", "English"]
tags: ["Linux", "Manjaro", "Gnome"]
slug: "customize-manjaro"
---

When I started to write the article, I haven't install the Chinese Input Method, and therefore I just completed it in English.

## Foreword

I've used Windows OS for a long time. I can still remember that time, when I was just a junior high school student. It's 2015. I was still a fan to Microsoft at that time. And it's also in that year, the Windows 10 was released. I was so excited but I didn't have a good PC then that could reach the lowest requirement of Windows 10. But I still tried to recommend one of my friends and helped him install this newly released operation system. At that time, I couldn't understand why there were so many people who did not want to try the new system, but kept the old and already-retired Windows XP. But many years have passed, and now it turns to me that I cannot understand why there are still so many bugs on Windows 10. So I was waiting for Windows 11 coming soon, hoping some breaking changes and improments. However, unfortunately, it is just not as good as what I thought.

About a month ago, on Jun 17th, I installed the Manjaro with Gnome.

But just after a month, on Jul 19th, it crashed. (but Windows is worse still I think)

And that's why I wrote this article ---- to restore my customized system working environment quickly instead of searching the bug solutions for a whole day for everytime I reinstalled the system.

## Customize

### Set Pacman Mirrors

```bash
sudo nano /etc/pacman.d/mirrorlist
```

Delete other unnecessary mirrors and add:

```
Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch
# Server = http://mirrors.redrock.team/manjaro/stable/$repo/$arch
```

Disable mirrorlist automatically refreshing:

```
sudo systemctl disable pamac-mirrorlist.timer
```

### Set the keyboard shortcuts

Open:

```bash
gnome-control-center
```

Go by `Keyboard -> Keyboard Shortcuts -> Customize Shortcuts`

|Name|Command|Shortcut|
|---|---|---|
|Launch Terminal|`gnome-terminal`|`Super` + `T`|
|Launch Nautilus|`nautilus`|`Super` + `E`|
|Delay Screenshot|`gnome-screenshot --delay 3`|`Ctrl` + `Print`|

### Edit the VSCode Settings

You need to installl VSCode before reading this section.

Edit `~/.config/Code/User/settings.json`:

```json5
{
    "window.zoomLevel": 2,
    // Fix Terminal Font Display
    "terminal.integrated.fontFamily": "NotoSansMono Nerd Font",
    // Make tab size from 2 to 4
    "vetur.format.options.tabSize": 4
}
```

### Enable Chrome Parallel Downloading

Open `chrome://flags/#enable-parallel-downloading`, and switch to `Enable`.

### Fix Multisystem time synchronization problem

```bash
timedatectl set-local-rtc 1
```

If you want to verify:

```bash
timedatectl
```

## Install Packages

### Basic

#### `base-devel`

The `base-devel` includes these packages:

`autoconf`, `automake`, `binutils`, `bison`, `fakeroot`, `file`, `findutils`, `flex`, `gawk`, `gcc`, `gettext`, `grep`, `groff`, `gzip`, `libtool`, `m4`, `make`, `pacman`, `patch`, `pkgconf`, `sed`, `sudo`, `texinfo`, `which`

If it's uninstalled, you may get some errors then like `ERROR: Cannot find the fakeroot binary.` when using yay. So run the command before you install other packages:

```bash
sudo pacman -S base-devel
```

#### Other Basic Packages

```bash
sudo pacman -S yay
sudo pacman -S vim
sudo pacman -S net-tools
```

### Tools

```bash
sudo pacman -S neofetch
sudo pacman -S btop
sudo pacman -S htop
sudo pacman -S gnome-terminal-fedora
sudo pacman -S wireshark-qt
sudo pacman -S aria2
sudo pacman -S gpick
yay -S marktext-bin
yay -S visual-studio-code-bin
yay -S obs-studio
yay -S google-chrome
yay -S firefox-developer-edition
yay -S feishu
# To install Netease Cloud Music (not official), run:
yay -S electron-netease-cloud-music
# Or a more beautiful version:
yay -S yesplaymusic
# Emoji Picker
yay -S emote
yay -S burpsuite
yay -S postman-bin
yay -S scrcpy
```

### Developing Environment

#### NodeJS

```bash
sudo pacman -S npm
sudo pacman -S yarn
sudo npm install -g nrm
nrm use taobao
sudo npm install -g @vue/cli
sudo npm install -g create-react-app
```

#### Deno

```bash
sudo pacman -S deno
```

#### Rust

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

#### Java

```bash
sudo pacman -S jdk-openjdk
```

### Install Docker

```bash
sudo pacman -S docker
sudo systemctl enable docker
```

### Install Asia Languages Input Methods

```bash
sudo pacman -S fcitx5 fcitx5-qt fcitx5-gtk
```
For Chinese:

```bash
sudo pacman -S fcitx5-rime
```

For Japanese:

```bash
sudo pacman -S fcitx5-mozc
```

For Korean:

```bash
sudo pacman -S fcitx5-hangul
```

Edit `~/.xprofile` for X11 users:

```bash
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS="@im=fcitx5"
```

Edit `~/.pam_environment` for Wayland users:

```bash
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS="@im=fcitx5"
```

Open ` fcitx5-configtool` to move the input methods from right that you are going to use to left.

To beautify the skin of the input methods, refer to [thep0y / fcitx5-themes](https://github.com/thep0y/fcitx5-themes).

### Install `icalingua`(Electron QQ)

```bash
yay -S mongodb-bin
yay -S mongodb-tools-bin
sudo systemctl enable mongodb.service
sudo systemctl start mongodb.service
yay -S icalingua
```

### Install `clash`

```bash
sudo pacman -S clash
```

Move your `config.yaml` to `~/.config/clash`. Then:

```bash
sudo systemctl enable clash@$USER
sudo systemctl start clash@$USER
```

Open the [dashboard](https://clash.razord.top/) for configuration.

