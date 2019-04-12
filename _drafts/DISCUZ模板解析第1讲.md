DISCUZ模板解析第1讲  
2009-02-11 17:49:35|  分类： PHP |字号 订阅
    




  下载LOFTER我的照片书  |
DISCUZ!(以后简称DZ)通过模板实现对整个程序皮肤的修改,我们常修改的几个模板,分别是header.htm(控制头 部),footer.htm(控制尾部),discuz.htm(控制首页中部),css.htm(CSS文档)以及6.0新增加的 css_append.htm(CSS新增文档)

本讲将重点讲述header.htm中的变量以及语句.
下面先介绍下变量的概念.
DISCUZ!模板中变量由”{“开头,”}”结尾,如{STYLEID},表示的是风格ID,此类变量多用于CSS定义中
它通常通过这些简单的特殊词句对一些较长,或者多次使用的字符进行一次定义,并且通过后台或者PHP程序文件中对变量的申明修改进行替换.变量都是由大写的唯一性词汇组成.

最简单的例子就是在风格中使用的新变量添加.
我们打开DZ的后台,论坛管理-界面风格-风格后的详情-最下端
[attach]1906[/attach]
当然.这个功能一般不常用.这里提出只是让大家能更加形象的了解变量这个概念.

另外有一种DZ的系统变量,通常是在PHP文件中申明的,通常以”$”开头,和上面的变量功能是一样的

下面我们开始从header.htm(以下简称header)最开始解说一些常用的变量及其中有的条件语句.
先是在<head>中的

<meta http-equiv="Content-Type" c />

其中的$charset表示编码类型,比如GBK,UTF-8

下一行的
复制内容到剪贴板
代码:
<title>$navtitle $bbname $seotitle - Powered by Discuz!</title>
其中的
$navtitle表示当前页面名称
$bbname表示站点名称
$seotitle 表示SEO名称

在往下面看,出现一段条件语句
复制内容到剪贴板
代码:
<!--{if $allowcsscache}-->
       <link rel="stylesheet" type="text/css" href="forumdata/cache/style_{STYLEID}.css" />
       <link rel="stylesheet" type="text/css" href="forumdata/cache/style_{STYLEID}_append.css" />
<!--{else}-->
       <style type="text/css">{template css}</style>
       <style type="text/css">{template css_append}</style>
<!--{/if}-->
这里表示的意思一句一句解释
<!--{if $allowcsscache}-->表示如果打开了CSS缓存(就是生成的.CSS文件)
那么...就加载以下两个CSS
forumdata/cache/目录下的...style_{STYLEID}.css和style_{STYLEID}_append.css(两个对应风格ID的CSS文档和CSS新增文档)
如果没有打开.则直接加载两个对应的CSS的模板
{template css}{template css_append}
其中template表示模板的意思,后面的就是模板名称,比如{template css}就是CSS.htm这个模板.
最后的<!--{/if}-->是结束语句
下面有段script,觉得有必要讲一下
复制内容到剪贴板
代码:
<script type="text/javascript">var IMGDIR = '{IMGDIR}';var attackevasive = '$attackevasive';var gid = 0;<!--{if in_array(CURSCRIPT, array('viewthread', 'forumdisplay'))}-->gid = parseInt('$thisgid');<!--{elseif CURSCRIPT == 'index'}-->gid = parseInt('$gid');<!--{/if}-->var fid = parseInt('$fid');var tid = parseInt('$tid');</script>
它的主要作用是把模板内的所有{IMGDIR}变为当前风格的图片路径.
比如{IMGDIR}/1.jpg,如果你在后台填写的风格图片路径是images/water,那么,这段语句在运行时,展现出来的路径就是images/water/1.jpg

接下来是我们最为熟悉的
复制内容到剪贴板
代码:
<a href="$indexname" title="$bbname">{BOARDLOGO}</a>
它代表的就是LOGO部分
$indexname表示站点路径
$bbname上面已经提到了.表示的是站点名称
{BOARDLOGO}变量代表的是站点的LOGO图片地址,替换的内容是(LOGO_url表示LOGO地址)
复制内容到剪贴板
代码:
<img src="LOGO_url" alt="$indexname">


第02部分将讲述从
复制内容到剪贴板
代码:
<div id="menu">
一直到header最后的部分
包括了最头部的难点:分栏条件语句以及菜单栏

下面我们接第01部分,继续讲解menu部分
在menu层下面一行
复制内容到剪贴板
代码:
<!--{if $_DCACHE['settings']['frameon'] > 0}-->
<span class="frameswitch">
<script type="text/javascript">
if(top == self) {
<!--{if ($_DCACHE['settings']['frameon'] == 2 && !defined('CACHE_FILE') && in_array(CURSCRIPT, array('index', 'forumdisplay', 'viewthread')) && (($_DCOOKIE['frameon'] == 'yes' && $_GET['frameon'] != 'no') || (empty($_DCOOKIE['frameon']) && empty($_GET['frameon']))))}-->
top.location = 'frame.php?frameon=yes&referer='+escape(self.location);
<!--{/if}-->
document.write('<a href="frame.php?frameon=yes" target="_top" class="frameon">{lang frameon_column}<\/a>');
} else {
document.write('<a href="frame.php?frameon=no" target="_top" class="frameoff">{lang frameon_flat}<\/a>');
}
</script>
</span>
<!--{/if}-->
这行就是分栏功能栏
复制内容到剪贴板
代码:
<!--{if $_DCACHE['settings']['frameon'] > 0}-->
表示如果分栏功能打开,是一个功能判断语句
下面接着的以下面语句开头
复制内容到剪贴板
代码:
<script type="text/javascript">
并以下面语句结尾
复制内容到剪贴板
代码:
</script>
使用了一个script脚本,书写了分栏定位以及用词判断
复制内容到剪贴板
代码:
if(top == self) {
<!--{if ($_DCACHE['settings']['frameon'] == 2 && !defined('CACHE_FILE') && in_array(CURSCRIPT, array('index', 'forumdisplay', 'viewthread')) && (($_DCOOKIE['frameon'] == 'yes' && $_GET['frameon'] != 'no') || (empty($_DCOOKIE['frameon']) && empty($_GET['frameon']))))}-->
top.location = 'frame.php?frameon=yes&referer='+escape(self.location);
<!--{/if}-->
这段表示了分栏打开的位置,也就是定位语句,模板书写中可以不必理会,知道功能是什么就行了.
下面的
复制内容到剪贴板
代码:
document.write('<a href="frame.php?frameon=yes" target="_top" class="frameon">{lang frameon_column}<\/a>');
} else {
document.write('<a href="frame.php?frameon=no" target="_top" class="frameoff">{lang frameon_flat}<\/a>');
}
说明书写的东西,如果打开分栏,则书写{lang frameon_column}变量,如果关闭分栏,显示{lang frameon_flat}变量,也就是菜单栏左部显示的关闭分栏,以及打开分栏.
最后以<!--{/if}-->结尾

下面就是所有的菜单右部,接着我们来一一解说
复制内容到剪贴板
代码:
<ul>
<!--{if $discuz_uid}-->
<li><cite><a href="space.php?action=viewpro&uid=$discuz_uid">$discuz_userss</a></cite></li>
<li><a href="$link_logout" class="notabs">{lang logout}</a></li>
<!--{if $maxpmnum}--><li<!--{if $BASESCRIPT == 'pm.php'}--> class="current"<!--{/if}-->><a href="pm.php" target="_blank">{lang pm}</a></li><!--{/if}-->
<!--{else}-->
<li<!--{if $BASESCRIPT == $regname}--> class="current"<!--{/if}-->><a href="$link_register" class="notabs">$reglinkname</a></li>
<li<!--{if $BASESCRIPT == 'logging.php'}--> class="current"<!--{/if}-->><a href="$link_login">{lang login}</a></li>
<!--{/if}-->

<!--{if $memliststatus}--><li<!--{if $BASESCRIPT == 'member.php'}--> class="current"<!--{/if}-->><a href="member.php?action=list">{lang memberlist}</a></li><!--{/if}-->
<!--{if $allowsearch || $qihoo['status']}--><li<!--{if $BASESCRIPT == 'search.php'}--> class="current"<!--{/if}-->><a href="search.php{if !empty($fid)}?srchfid=$fid{/if}">{lang search}</a></li><!--{/if}-->
<!--{if $tagstatus}--><li<!--{if $BASESCRIPT == 'tag.php'}--> class="current"<!--{/if}-->><a href="tag.php">{lang tag}</a></li><!--{/if}-->
<!--{if !empty($plugins['links'])}-->
<!--{loop $plugins['links'] $module}-->
<!--{if !$module['adminid'] || ($module['adminid'] && $adminid > 0 && $module['adminid'] >= $adminid)}--><li>$module[url]</li><!--{/if}-->
<!--{/loop}-->
<!--{/if}-->
<!--{if $discuz_uid}-->
<!--{if $jsmenu[4]}--><li id="my" class="dropmenu<!--{if $BASESCRIPT == 'my.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="my.php">{lang my}</a></li><!--{else}--><li><a href="my.php?item=threads"<!--{if $BASESCRIPT == 'my.php'}-->class="current"<!--{/if}-->>{lang show_mytopics}</a></li><li><a href="my.php?item=grouppermission">{lang my_permissions}</a></li><!--{/if}-->
<!--{if $jsmenu[2]}--><li id="memcp" class="dropmenu<!--{if $BASESCRIPT == 'memcp.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="memcp.php">{lang memcp}</a></li><!--{else}--><li><a href="memcp.php"<!--{if $BASESCRIPT == 'memcp.php'}-->class="current"<!--{/if}-->>{lang memcp}</a></li><!--{/if}-->
<!--{if $regstatus > 1}--><li<!--{if $BASESCRIPT == 'invite.php'}--> class="current"<!--{/if}-->><a href="invite.php">{lang invite}</a></li><!--{/if}-->
<!--{if $magicstatus}--><li<!--{if $BASESCRIPT == 'magic.php'}--> class="current"<!--{/if}-->><a href="magic.php">{lang magics_title}</a></li><!--{/if}-->
<!--{/if}-->
<!--{if !empty($plugins['jsmenu'])}--><li id="plugin" class="dropmenu" onmouseover="showMenu(this.id)"><a href="#">$pluginjsmenu</a></li><!--{/if}-->
<!--{if $allowviewstats}--><!--{if !empty($jsmenu[3])}--><li id="stats" class="dropmenu<!--{if $BASESCRIPT == 'stats.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="stats.php">{lang statistics}</a></li><!--{else}--><li><a href="stats.php">{lang statistics}</a></li><!--{/if}--><!--{/if}-->
<!--{if $discuz_uid && in_array($adminid, array(1, 2, 3))}--><li><a href="admincp.php" target="_blank">{lang admincp}</a></li><!--{/if}-->
<li<!--{if $BASESCRIPT == 'faq.php'}--> class="current"<!--{/if}-->><a href="faq.php">{lang faq}</a></li>
</ul>
先是这段
复制内容到剪贴板
代码:
<!--{if $discuz_uid}-->
<li><cite><a href="space.php?action=viewpro&uid=$discuz_uid">$discuz_userss</a></cite></li>
<li><a href="$link_logout" class="notabs">{lang logout}</a></li>
<!--{if $maxpmnum}--><li<!--{if $BASESCRIPT == 'pm.php'}--> class="current"<!--{/if}-->><a href="pm.php" target="_blank">{lang pm}</a></li><!--{/if}-->
<!--{else}-->
<li<!--{if $BASESCRIPT == $regname}--> class="current"<!--{/if}-->><a href="$link_register" class="notabs">$reglinkname</a></li>
<li<!--{if $BASESCRIPT == 'logging.php'}--> class="current"<!--{/if}-->><a href="$link_login">{lang login}</a></li>
<!--{/if}-->
里面的第一句
复制内容到剪贴板
代码:
<!--{if $discuz_uid}-->
说的是登陆判断,其实就是如果有DISCUZ的UID,即登陆状态,则显示
复制内容到剪贴板
代码:
<li><cite><a href="space.php?action=viewpro&uid=$discuz_uid">$discuz_userss</a></cite></li>
<li><a href="$link_logout" class="notabs">{lang logout}</a></li>
<!--{if $maxpmnum}--><li<!--{if $BASESCRIPT == 'pm.php'}--> class="current"<!--{/if}-->><a href="pm.php" target="_blank">{lang pm}</a></li><!--{/if}-->
第一个<li>里是$discuz_userss用户名,连接到"space.php?action=viewpro&uid=$discuz_uid",也就是空间连接.
第二个<li>里是{lang logout},连接到的是$link_logout,即登出连接
第三个是一段变量代码,变量是{lang pm},也就是短消息,这里有<!--{if $BASESCRIPT == 'pm.php'}--> class="current"<!--{/if}-->在下面也会出现,说的意思是,如果定位的URL是pm.php,则显示 current这个CLASS,大家在DST点到短消息,然后看看菜单就知道这个class是什么样子了
"<!--{else}-->"这个语句非常常见,相当于C语言以及其他程序语言中的else,和if搭配使用.
这里说明的意思是,如果不是登陆状态,则显示下面的内容.
复制内容到剪贴板
代码:
<li<!--{if $BASESCRIPT == $regname}--> class="current"<!--{/if}-->><a href="$link_register" class="notabs">$reglinkname</a></li>
<li<!--{if $BASESCRIPT == 'logging.php'}--> class="current"<!--{/if}-->><a href="$link_login">{lang login}</a></li>
这里的判断语句上面短消息部分已经说过,所以不再说明,下面出现相同部分也不再说明,请各位参考短消息举一反三,这里出现的$reglinkname以及{lang login}就是注册,登陆的意思了.
下面的
复制内容到剪贴板
代码:
<!--{if $memliststatus}--><li<!--{if $BASESCRIPT == 'member.php'}--> class="current"<!--{/if}-->><a href="member.php?action=list">{lang memberlist}</a></li><!--{/if}-->
这里主要说下<!--{if $memliststatus}-->这个判断,意思是如果会员列表打开,{lang memberlist}就是会员列表的变量
复制内容到剪贴板
代码:
<!--{if $allowsearch || $qihoo['status']}--><li<!--{if $BASESCRIPT == 'search.php'}--> class="current"<!--{/if}-->><a href="search.php{if !empty($fid)}?srchfid=$fid{/if}">{lang search}</a></li><!--{/if}-->
这个是搜索打开的变量,这里的<!--{if $allowsearch || $qihoo['status']}-->表示,$allowsearch是允许搜索.||是连接词,表示或者的意 思.$qihoo['status']是奇虎搜索.{lang search}是搜索的变量,不常用到,也就只说这么点了.
复制内容到剪贴板
代码:
<!--{if $tagstatus}--><li<!--{if $BASESCRIPT == 'tag.php'}--> class="current"<!--{/if}-->><a href="tag.php">{lang tag}</a></li><!--{/if}-->
这个是标签
复制内容到剪贴板
代码:
<!--{if !empty($plugins['links'])}-->
<!--{loop $plugins['links'] $module}-->
<!--{if !$module['adminid'] || ($module['adminid'] && $adminid > 0 && $module['adminid'] >= $adminid)}--><li>$module[url]</li><!--{/if}-->
<!--{/loop}-->
<!--{/if}-->
这个是插件,是平铺插件时候显示的循环
复制内容到剪贴板
代码:
<!--{if $discuz_uid}-->
<!--{if $jsmenu[4]}--><li id="my" class="dropmenu<!--{if $BASESCRIPT == 'my.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="my.php">{lang my}</a></li><!--{else}--><li><a href="my.php?item=threads"<!--{if $BASESCRIPT == 'my.php'}-->class="current"<!--{/if}-->>{lang show_mytopics}</a></li><li><a href="my.php?item=grouppermission">{lang my_permissions}</a></li><!--{/if}-->
这个是调用JS命令,控制菜单里"我的"部分选项
复制内容到剪贴板
代码:
<!--{if $jsmenu[2]}--><li id="memcp" class="dropmenu<!--{if $BASESCRIPT == 'memcp.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="memcp.php">{lang memcp}</a></li><!--{else}--><li><a href="memcp.php"<!--{if $BASESCRIPT == 'memcp.php'}-->class="current"<!--{/if}-->>{lang memcp}</a></li><!--{/if}-->
这个是调用JS命令,控制菜单里的控制面板选项
复制内容到剪贴板
代码:
<!--{if $regstatus > 1}--><li<!--{if $BASESCRIPT == 'invite.php'}--> class="current"<!--{/if}-->><a href="invite.php">{lang invite}</a></li><!--{/if}-->
这个是邀请注册
复制内容到剪贴板
代码:
<!--{if $magicstatus}--><li<!--{if $BASESCRIPT == 'magic.php'}--> class="current"<!--{/if}-->><a href="magic.php">{lang magics_title}</a></li><!--{/if}-->
这个是道具
复制内容到剪贴板
代码:
<!--{if !empty($plugins['jsmenu'])}--><li id="plugin" class="dropmenu" onmouseover="showMenu(this.id)"><a href="#">$pluginjsmenu</a></li><!--{/if}-->
这个是插件,下拉菜单菜单时候的显示
复制内容到剪贴板
代码:
<!--{if $allowviewstats}--><!--{if !empty($jsmenu[3])}--><li id="stats" class="dropmenu<!--{if $BASESCRIPT == 'stats.php'}--> current<!--{/if}-->" onmouseover="showMenu(this.id)"><a href="stats.php">{lang statistics}</a></li><!--{else}--><li><a href="stats.php">{lang statistics}</a></li><!--{/if}--><!--{/if}-->
这个是统计
复制内容到剪贴板
代码:
<!--{if $discuz_uid && in_array($adminid, array(1, 2, 3))}--><li><a href="admincp.php" target="_blank">{lang admincp}</a></li><!--{/if}-->
这个是系统设置,也就是后台
复制内容到剪贴板
代码:
<li<!--{if $BASESCRIPT == 'faq.php'}--> class="current"<!--{/if}-->><a href="faq.php">{lang faq}</a></li>
这个是帮助