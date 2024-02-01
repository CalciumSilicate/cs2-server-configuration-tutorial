
# CS2社区竞技服务器配置

> 目的：开设服务器进行好友之间的内战

> 此教程使用的操作系统为[Archlinux](https://archlinux.org/)

> 由于[Valve](https://www.valvesoftware.com/)并没有推出官方的[CS2](https://store.steampowered.com/app/730/CounterStrike_2/)的社区服务器，且没有在CS2可用的[Get5](https://github.com/splewis/get5)与[Sourcemod](https://www.sourcemod.net/)，所以这个方案随时可能过时。
> 替代Get5的方案是使用[MatchZy](https://github.com/shobhit-pathak/MatchZy)，还有很多不完善的地方，所以说还是等~~Vacation~~Valve的社区服务器吧。

---
# I. 安装CS2与Matchzy
## 1.1 安装CS2

> 由于Valve并没有推出官方的CS2社区服，所以这一步相当于安装CS2游戏本体

安装SteamCMD并启动
```shell
$ yay -S steamcmd
$ steamcmd
```

安装CS2
```shell
Steam>login anonymous # 匿名登录
Steam>app_update 730 validate
```

完成后退出
```shell
Steam>exit
```

启动cs2服务器
```shell
$ cd ~/.steam/SteamApps/common/Counter-Strike Global Offensive/game/bin/linuxsteamrt64
$ ./cs2 -dedicated +map de_mirage
```

如果出现以下报错：
```text
steamclient.so: cannot open shared object file: No such file or directory
dlopen failed trying to load:
/home/arch/.steam/sdk64/steamclient.so
with error:
steamclient.so: cannot open shared object file: No such file or directory
dlopen failed trying to load:
/home/arch/.steam/sdk64/steamclient.so
with error:
/home/arch/.steam/sdk64/steamclient.so: cannot open shared object file: No such file or directory
[S_API] SteamAPI_Init(): Failed to load module '/home/arch/.steam/sdk64/steamclient.so'
Failed to initialize Steamworks SDK for gameserver.  Failed to load module '/home/arch/.steam/sdk64/steamclient.so'
 0 Failed to initialize Steamworks SDK for gameserver.  Failed to load module '/home/arch/.steam/sdk64/steamclient.so'
```

说明缺少了steamclient.so，可以用以下方法解决：
```shell
$ mkdir -p ~/.steam/sdk64/
$ steamcmd +login anonymous +app_update 1007 +quit
$ cp ~/.steam/SteamApps/common/Steamworks\ SDK\ Redist/linux64/steamclient.so ~/.steam/sdk64/
```

再启动cs2服务器就没有问题了
```shell
$ ./cs2 -dedicated +map de_mirage
```

## 1.2 安装Metamod

到[Metamod](https://www.sourcemm.net/downloads.php?branch=master&all=1)官网去下载Linux版本
```shell
$ wget https://mms.alliedmods.net/mmsdrop/2.0/mmsource-x.x.x-git1280-linux.tar.gz
```

解压
```shell
$ tar -zxvf mmsource-x.x.x-git1280-linux.tar.gz
```

将得到的Addons 文件夹放置在csgo目录下
```
$ cp -r addons ~/.steam/SteamApps/common/Counter-Strike\ Global\ Offensive/game/csgo/
```

修改`.steam/SteamApps/common/Counter-Strike Global Offensive/game/csgo/gameinfo.gi`，在`FileSystem{SearchPaths{`中添加`Game csgo/addons/metamod`
> 提示: vim 中在`insert`模式下使用Ctrl+v+i可以输入制表符

修改后如下
```cfg
...
  FileSystem
  {
    SearchPaths
    {
      Game_LowViolence> csgo_lv
	  
+     Game csgo/addons/metamod
      
	  Game> csgo
	  Game> csgo_imported
	  ...
...
```

>每次更新服务端后，都要再修改一次这个文件

## 1.3 安装MatchZy

>MatchZy是类似于get5的一款插件，可以进行竞技对局的控制

前往[Releases](https://github.com/shobhit-pathak/MatchZy/releases/)页面下载带有`with-cssharp`字样的压缩包，并解压缩
```shell
$ wget https://github.com/shobhit-pathak/MatchZy/releases/download/0.6.1-alpha/matchzy-0.6.1-with-cssharp.zip
$ unzip matchzy-0.6.1-with-cssharp.zip
```

将得到的两个文件夹复制到csgo目录下
```shell
$ cp -r addons ~/.steam/SteamApps/common/Counter-Strike\ Global\ Offensive/game/csgo/
$ cp -r cfg ~/.steam/SteamApps/common/Counter-Strike\ Global\ Offensive/game/csgo/
```

根据Matchzy的教程进行[配置](https://shobhit-pathak.github.io/MatchZy/configuration/)

要配置管理员，编辑`~/.steam/SteamApps/common/Counter-Strike\ Global\ Offensive/game/csgo/cfg/MatchZy/admins.json`
```json
{
	"<steam_id>": ""
}
```

steam_id为个人steam主页链接中一串数字`https://steamcommunity.com/profiles/<steam_id>/home/`
## 1.4 启动并连接

启动CS2服务器
```shell
$ cd ~/.steam/SteamApps/common/Counter-Strike Global Offensive/game/bin/linuxsteamrt64
$ ./cs2 -dedicated +map de_mirage
```

开放27015端口与27020端口（用于GOTV）

游戏内控制台输入进行连接：
```
connect x.x.x.x:xxxx
```

## 1.5 使用MatchZy的命令

参考[MatchZy Usage Commands](https://shobhit-pathak.github.io/MatchZy/commands/)
# II. 安装Get5 Panel

>Get5 Panel分为前后端两部分，即[G5V](https://github.com/PhlexPlexico/G5V) 与 [G5API](https://github.com/PhlexPlexico/G5API)，可以在网页中对比赛进行控制

明天再写
