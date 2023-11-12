# Prevent Wechat Shadow

A script to hide the annoying shadow of Wechat running on WINE

---

**THIS SCRIPT IS BASED ON THIS ARTICLE:
[ZHIHU - *wine-wechat 窗口阴影置顶解决方案* by Reverier](https://zhuanlan.zhihu.com/p/106926984)**


## Usage
Execute the script when Wechat is opened

## Pre-requisites
- Dependences should be checked before the first running

- `dpiScale` should be **verified and modified** to suit the desktop environment

- `checkInterval` can be altered to change the check interval


## Dependencies
- `xwininfo` 

   -  To get the id of shadow-like window

- `xdotool`

    - To hide the shadow-like window


## How does it work

the whole process is as follows:

- Check the dependencies

    - It uses `whereis` command to check if `xwininfo` and `xdotool` is installed

- Check if wechat is opened

    - It uses the command `ps -ef | grep WeChat.exe | grep -v "grep" | grep -v "ps"` to get the process information which contains `WeChat.exe`

    - The process names which contain `ps` and `grep` patterns are ignored (these process is used to check if wechat is opened)

    - If there's still some information about wechat, it means wechat is opened

- Get the id of shadow-like window

    - It uses the command `xwininfo -tree -root | grep "wechat" | grep "(has no name)" | grep -v "1x1+0+0  +0+0"` to get all the shadow-like windows

        - `xwininfo -tree -root` is used to list all of the windows

        - command `grep "wechat" | grep "(has no name)" | grep -v "1x1+0+0  +0+0"` is used to filter the shadow-like windows
            - `wechat`: get the windows opened by wechat
            - `(has no name)`: the shadows are always has no name
            - `-v 1x1+0+0  +0+0`: the shadow must has a considerable size, otherwise it cannot be a shadow

    - Filter the menus

        - Window filtered contains both menus and shadows. 
        
        - So it's necessary to compare the size of the windows with the given size (`maxMenuSize`) of the common menus
        
        - If the window is smaller than the given size, it may be a menu and should be ignored

- Hide the shadow-like windows

    - It uses `xdotool windowunmap <window id>` to hide the shadow-like windows

---

**NOTICE:** This script can effectively prevent the shadow on *my computer* (`manjaro, xfce4, X11, Python 3.11.5, wine 8.18`) with `wechat 3.2.1`. I **can not assure** it will work on other desktop environments with other versions of wechat.

---

LAST UPDATE: 2023-11-12