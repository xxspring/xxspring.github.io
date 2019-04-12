---
layout:     post
title:      "MAC升级Nodejs和Npm到最新版"
subtitle:   ""
date:       2019-04-10 16:00:00
author:     "xxspring"
header-img: "img/post-bg-2015.jpg"
tags:
    - Mac
---

## MAC升级Nodejs和Npm到最新版

第一步，先查看本机node.js版本：

```
node -v
```

第二步，清除node.js的cache：

```
sudo npm cache clean -f
```

第三步，安装 n 工具，这个工具是专门用来管理node.js版本的，别怀疑这个工具的名字，是他是他就是他，他的名字就是 "n"

```
sudo npm install -g n
```

第四步，安装最新版本的node.js

```
sudo n stable
```

第五步，再次查看本机的node.js版本：

```
node -v
```

第六步，更新npm到最新版：

```
$ sudo npm install npm@latest -g
```

第七步，验证

```
node-vnpm -v
```

![截图](/img/screen/2019-04-11-MAC升级Nodejs和Npm到最新版.png "截图")
