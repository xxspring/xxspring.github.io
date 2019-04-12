discuz 文件结构   
2009-02-06 14:59:22
    


前言：为方便互联网数万Discuz!爱好者，更加深入了解Discuz!软件，本人在熟悉Discuz!过程中，顺便将个人经验写给大家。“Discuz!”在下文中简称 “DZ”。要弄DZ二次开发，必须至少具备如下技能：
1） 能够理很好理解MVC构架的原理(虽然DZ不是MVC架构的)
2） 扎实的PHP基础，熟悉结构化程序，OOP程序的写法及应用
3） 熟悉MYSQL就用，掌握SQL语言，懂SQL优化者更佳
4） 熟悉使用Discuz!的各项功能

一） Discuz!的文件系统目录
注：想搞DZ开发，就得弄懂DZ中每个文件的功能。
a) Admin：后台管理功能模块
b) Api：DZ系统与其它系统之间接口程序
c) Archiver：DZ中，用以搜索引擎优化的无图版
d) Attachments：DZ中 ,用户上传附件的存放目录
e) Customavatars：DZ中，用户自定义头像的目录
f) Forumdata：DZ缓存数据的存放目录
g) Images：DZ模板中的图片存放目录
h) Include：DZ常用函数库，基本功能模块目录
i) Ipdata：DZ统计IP来路用的数据
j) Plugins：DZ插件信息的存放目录
k) Templates：DZ模板文件的存放目录
l) Wap：DZ无线，Wap程序处理目录

二） 必须记熟Discuz!数据库设计的每个表的功能，每个表中每个字段的功能。
关于DZ数据库设计文档，请参阅DZ相关的项目文档(请从本贴附件中下载)

三） Discuz!的流程控制
a) 后台流程控：DZ后台所有的功能，均需要注册到admincp.php文件，每个功能都至少有一个或一个以上的Action(动作)，在 admincp.php中，可以定义Action的执行权限，分别为：“admin==1”管理员，或“admin==2 || admin==3”超级版主和版主，每个Action对应一个脚本文件，脚本文件的命名为action.inc.php(*.inc.php)，并存放在 admin目录下，如执行：admincp.php?action=dodo，相当于执行admin目录下的dodo.inc.php文件
b) 前台流程控制：前台的流程控制比较简单：流程是自由的，如：
首页：index.php
会员注册：register.php；
会员登录：logging.php
发贴程序：post.php
会员信息：member.php
论坛内容：forumdisplay.php
查看贴子：viewthread.php
…大部分功能，此处不一一列出…
c) DZ根目下的config.inc.php属于整个DZ系统的配置文件

四） Discuz!的数据处理过程
a) DZ对mysql的数据库操作处理全部封装在dbstuff(db_mysql.class.php)类中
b) 所在的外部数据均通过“daddslashes()”初步过滤，然后再过滤，再根据需要处理

五） Discuz!的显示控制(网站多样式风格输出)
a) 显示层就是大家通常所看到的网站风格了。DZ中每套风络分别在templates及images下对应一个风格文件的存放目录。网站风格的制作，请参阅详细的DZ风格制作文档
b) DZ网站风格文件处理的原理：其实很简单，DZ使用template.func.php中的parse_template()以PHP正则运算把htm模 文件中的模板标签，转换成了PHP代码，并根据styleid保存在forumdata/templates下，这个有点像Smarty中的技术。

六） DZ中的语言处理
a) DZ前台及后台中、英语言的实现，均是把语句定义成了语变量，然后在模板输入，语句变量的赋值，均放在模板目录中的*.lang.php文件中，DZ在生成网站风格时就加载了这相应的语言包。

七） DZ如何处理用户信息(存取、计算、更新过程)
新手要做二次开发，都必须掌握这数组中，每个数组元素的意义。
a) DZ的基本信息，如用户信息，Session信息存在如下变量中：
a). $_DCACHE
b). $_COOKIE
c). $_DCOOKIE
d). $_DSESSION
e). $_DPLUGIN
b) 可以通过print_r($GLOBALS)，打印全部变量

八） DZ中缓存处理机制
a) DZ中缓存处理过程都放在“cache.func.php”中，DZ的缓存处理比较简单，其原理是把一个数组转换成了PHP代码，并保存在缓存目录下，大家可打开缓存文件查看便知。
b) 使用方法：如果在新开的功能中，需要缓存某部分数据，基本上就是：
1）定义并注册缓存名字。
2）从数据读取相应的数据。
3）数据在写入缓存前作相应处理。
4）最后写入缓存。
具体操作，可以看文件中的代码，做相应的修改即可

九） DZ中模板处理机制
a) DZ独创的模板处理技术，类似于Smarty中的模板处理，只是具体算法，过程不同，Smarty是一种重型模板引擎方案。其原理都是把模板中的变量转换成相应的PHP代码，这个过程实际是模访JAVA中的一次编译，多处运行。

十） DZ中权限处理机制
a) 对于DZ中前台的每相action都有$discuz_action定义，DZ根据用户所在的用户组来判定用户是否具有相应操作$discuz_action的权限。至于后台的权限权验证，则更简单了，依据“admin==1”来确定的

十一） DZ中如何实现URL静态化
a) DZ中的静态有两法，只要懂ReWrite规划的朋友，一看就知。

十二） DZ独创的HTML编辑器，如何截取并使用，如果进行Discuz!代和Html代码的转换
a) 这也算是DZ比较牛的一项技术了，在早期版中，因DZ编辑器的不足，使得很多用户放弃了DZ。实现原理：通过JS把用的一些操作转换成了DZ的 bbcode代码。这样子提交了安全性，将带有bbcode代码的内容存入数据，在用户打开页页时，又把bbcode代码转换成html代码
本贴声明：由于时间有限，本贴只有关于DZ部分功能的简短分析。若各位网友，对本文感兴趣并想更为深入了解DZ，请在本贴后回贴！我将尽可能多的DZ技术分析写在本文，不断更新本贴内容。
部分文件说明:
admincp.php 管理
ajax.php ajax功能
announcement.php 公告
attachment 附件
board.php 真正的首页
config.inc.php 这个是配置文件
corpus.php 论坛文集
digest.php 精华帖子
discuz_version.php 论坛版本号
faq.php 问题列表
forumdisplay.php 论坛列表
index.php 跳转页面
loggin.php 认证页面(登录退出)
mail_config.inc.php 邮件配置
member.php 用户操作
memcp.php 个人控制面版
misc.php 零碎功能
my.php 我的帖子
plugin.php 插件
pm.php 短信
post.php 发送帖子
redirect.php 页面重定向
register.php 注册
robots.txt 限制搜索
rss.php rss信息发布
search.php 论坛查询
secode.php 验证码
stats.php 统计
topic.php 首页论坛专题
topicadmin 主题管理
viewpro.php 显示个人信息
viewthread.php 主题显示
文件夹
admin 管理
api 接口
archiver 文档
attachments 附件
customavatars 自定义表情
forumdata 论坛数据包含缓冲数据
images 图片
include 公共文件
install 安装包
ipdata ip地址
plugins 插件
readme 帮助文档
templates 模板
utilities 工具包
wap 手机网站
文件夹include
advertisements.inc.php 广告管理
ajax.js ajax相关
attachment.func.php 附件函数集
bbscode.js 论坛表情
cache.fun.php 缓存函数集
category.inc.php 栏目
chinese.class.php
common.inc.php 最主要的头文件
common.js 最主要的js文件
corpus.func.php 论坛文集函数
counter.inc.php 论坛计数
cron.func.php 计划任务
db_mysql.class.php 数据库
db_mysql_error.inc.php 数据库错误
debug.php 调试信息
discuzcode.func.php 论坛代码
editor.func.php 编辑器
editor.js 编辑器
editpost.inc.php 编辑帖子
floatadv.js 浮动广告
forum.func.php 论坛函数集
global.func.php 全局函数
menu.js 菜单
misc.func.php 其它
newreply.inc.php 新回复
newthread.inc.php 新主题
*pmprompt.inc.php
post.fun.php 发表主题
printable.inc.php 论坛打印
qihoo.js qihoo
relatethreads.inc.php 相关主题
security.inc.php 安全
sendmail.inc.php 邮件
serverbusy.htm 系统繁忙
template.func.php 模板
threadpay.inc.php 购买帖子

6.0 结构

 

管理程序（后台）： admincp.php 实际上是通过调用admin文件夹下面 *.inc.php 程序实现各个功能模块      
AJAX功能：    ajax.php
论坛公告：    announcement.php
附件相关：    attachment.php
blog.php
配置文件：    config.inc.php
      crossdomain.xml
精华帖子：    digest.php
论坛版本：    discuz_version.php
      eccredit.php
论坛Faq（问题）： faq.php
内容（板块）列表： forumdisplay.php
frame.php
首页:     index.php
安装程序：    install.php
      invite.php
左右分栏：    leftmenu.php
登入登出程序：   logging.php
道具商店：    magic.php
会员信息：    member.php
会员控制面板：   memcp.php
零碎功能：    misc.php
提供双击编辑帖子： modcp.php
【我的】功能：   my.php
插件程序：    plugin.php
论坛短信息：   pm.php
发帖程序：    post.php
页面重定向(跳转)： redirect.php
注册程序：    register.php
relatekw.php
回复帖子：    relatethread.php
RSS定制：    rss.php
论坛那搜索：   search.php
验证码程序：   seccode.php
论坛地图：    sitemap.php
空间：     space.php
统计:     stats.php
标签：     tag.php
首页论坛专题：   topic.php
主题管理     topcadmin.php
trade.php
会员个人信息：   viewpro.php
查看帖子：    viewthread.php
文件夹：
admin 管理文件夹【包含后台各个模块功能】
api 接口文件夹【提供和其他CMS的接口】
archiver 静态无图文档文件夹
attachments 附件【前台上传的各种文件】
customavatars 自定义表情
forumdata 论坛备份数据和缓存
images 图片【论坛图片包括各个模板用到的图片】
include 公共文件夹【dz的核心程序都在里面】
install 安装包
ipdata ip地址
plugins 插件文件夹【标准插件】
templates 模板文件夹
wap 手机网站文件夹

到dz6.0后 对文件夹进行了归类 将 js 和php 分开放置
JS 统一放在 javascript 文件夹里
个人目前 discuz 的命名规则四类文件：
.php 呵呵，直接是程序咯~
.inc.php   是相关【功能文件】 比如：newthread.inc.php 是 新主题 相关的功能
.func.php 是相关【函数文件】 比如：global.func.php 是 全局函数
.class.php 是【类文件】 比如： 对mysql操作就放在 db_mysql.class.php 文件里
+【文件夹include 】
+【crons文件夹】
   announcements_daily.inc.php
   birthdays_daily.inc.php
   cleanup_daily.inc.php
   cleanup_monthly.inc.php
   magics_daily.inc.php
   notify_daily.inc.php
   onlinetime_monthly.inc.php
   promotions_hourly.inc.php
   secqaa_daily.inc.php
   supe_daily.inc.php
   tags_daily.inc.php
   threadexpiries_hourly.inc.php
   todayposts_daily.inc.php
+【javascript文件夹】
   ajax.js
   bbcode.js
   calender.js
   common.js
   drag.js
   drag_space.js
   editor.js
   floatadv.js
   google.js
   iframe.js
   insenz_reg.js
   menu.js
   msn.js
   post.js
   post_attach.js
   qihoo.js
   tree.js
   viewthread.js
+【magic文件夹】
   magic_close.inc.php
   magic_color.inc.php
   magic_del.inc.php
   magic_hidden.inc.php
   magic_money.inc.php
   magic_move.inc.php
   magic_open.inc.php
   magic_renew.inc.php
   magic_reporter.inc.php
   magic_see.inc.php
   magic_top.inc.php
   magic_up.inc.php
+【tables文件夹】
   big5-unicode.table
   gb-unicode.table
  
advertisements.inc.php
attachment.func.php   附件函数集
cache.func.php    缓存函数集
category.inc.php   栏目
chinese.class.php
common.inc.php    最主要的头文件
counter.inc.php    论坛计数
cron.func.php    计划任务
db_mysql.class.php   数据库
db_mysql.error.inc.php 数据库错误
discuzcode.func.php   论坛代码
ec_credit.func.php
editor.func.php    编辑器
editpost.inc.php   编辑帖子
forum.func.php    论坛函数集
ftp.func.php
gifmerge.class.php
global.func.php    全局函数
image.class.php
insenz.func.php
insenz_cron.func.php
magic.func.php   
misc.func.php    其它
moderatio.inc.php
newrepley.inc.php   新回复
newthread.inc.php   新主题
newtrade.inc.php   新活动
pprompt.inc.php
post.func.php    发表主题
printable.inc.php   论坛打印
promotion.inc.php
search_qihoo.inc.php
search_trade.inc.php
search_type.inc.php
security.inc.php   安全
sendmail.inc.php   邮件
serverbusy.htm    系统繁忙
space.func.php
supesite.func.php
supesite_circle.inc.php
supesite_import.inc.php
temple.func.php    模板
treadpay.inc.php   购买帖子
viewpro.inc.php    个人信息
viewthread_activity.inc.php 活动主题
viewthread_debate.inc.php   辩论主题
viewthread_poll.inc.php    投票主题
viewthread_reward.inc.php   悬赏主题
viewthread_special.inc.php   活动主题
viewthread_trade.inc.php   商品主题
viewthread_video.inc.php   视频主题
xmlparser.class.php