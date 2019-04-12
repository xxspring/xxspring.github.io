DISCUZ模板解析第2讲  
2009-02-11 17:50:09|  分类： PHP |字号 订阅
    




  下载LOFTER我的照片书  |
从最上面的闭合</div>开始,就是footer.htm了.

下面的
引用:
<!--{if !empty($jsmenu) && (empty($bbclosed) || $adminid == 1)}-->{template jsmenu}<!--{/if}-->
是一段JSmenu开启的条件语句,在制作风格的时候记住这个不能遗漏了
引用:
<div id="ad_footerbanner1"></div><div id="ad_footerbanner2"></div><div id="ad_footerbanner3"></div>
这个就是大家所知道的广告栏了

<div id="footer">是底部的框架开始
下面的{lang time_now}是现在时间的变量
引用:
<!--{if $icp}--> <a href="http://www.miibeian.gov.cn/" target="_blank">$icp</a><!--{/if}-->
这个是ICP备案的条件语句.

接下的依次是{lang clear_cookies}清除cookies,$sitename站点名称
引用:
<!--{if $archiverstatus}--> - <a href="archiver/" target="_blank">Archiver</a><!--{/if}-->
开启Archiver(对浏览器的简易模式)的条件语句
引用:
<!--{if $wapstatus}--> - <a href="wap/" target="_blank">WAP</a><!--{/if}-->
开启WAP站点的条件语句
引用:
<span id="styleswitcher" class="dropmenu" >{lang style}</span>
                                    <script type="text/javascript">
                                    function setstyle(styleid) {
                                    <!--{if CURSCRIPT == 'forumdisplay'}-->
                                             location.href = 'forumdisplay.php?fid=$fid&page=$page&styleid=' + styleid;
                                    <!--{elseif CURSCRIPT == 'viewthread'}-->
                                             location.href = 'viewthread.php?tid=$tid&page=$page&styleid=' + styleid;
                                    <!--{else}-->
                                             location.href = '$indexname?styleid=' + styleid;
                                    <!--{/if}-->
                                    }
                                    </script>
<div id="styleswitcher_menu" class="popupmenu_popup" style="display: none;">
                                    <ul>
                                             <!--{loop $stylejump $id $name}-->
                                             <li{if $id == $styleid} class="current"{/if}><a href="###" >$name</a></li>
                                             <!--{/loop}-->
                                    </ul>
                                    </div>
                               <!--{/if}-->
这段代码是风格选择的HTML以及JS代码,我们依次解说.
<span id="styleswitcher" class="dropmenu" >{lang style}</span>
中的 是一段被定义的动作,代表鼠标移过,则显示本ID的menu部分,比如这个的ID是styleswitcher,则移过它显示了一个新的层,ID是styleswitcher_menu

也就是下面的
引用:
<div id="styleswitcher_menu" class="popupmenu_popup" style="display: none;">
                                    <ul>
                                             <!--{loop $stylejump $id $name}-->
                                             <li{if $id == $styleid} class="current"{/if}><a href="###" >$name</a></li>
                                             <!--{/loop}-->
                                    </ul>
                                    </div>
这段代码
引用:
       <script type="text/javascript">
                                    function setstyle(styleid) {
                                    <!--{if CURSCRIPT == 'forumdisplay'}-->
                                             location.href = 'forumdisplay.php?fid=$fid&page=$page&styleid=' + styleid;
                                    <!--{elseif CURSCRIPT == 'viewthread'}-->
                                             location.href = 'viewthread.php?tid=$tid&page=$page&styleid=' + styleid;
                                    <!--{else}-->
                                             location.href = '$indexname?styleid=' + styleid;
                                    <!--{/if}-->
                                    }
                                    </script>
这段JS,定义了在不同位置转换风格的动作

接下来有几个小变量,$version是版本变量
引用:
<!--{if debuginfo()}-->
                     <p id="debuginfo">;Processed in $debuginfo[time] second(s), $debuginfo[queries] queries<!--{if $gzipcompress}-->, Gzip enabled<!--{/if}-->
这个是Gzip的传输数据条件语句

下面的
引用:
<!--{if $_DCACHE['settings']['frameon'] && in_array(CURSCRIPT, array('index', 'forumdisplay', 'viewthread')) && $_DCOOKIE['frameon'] == 'yes'}-->
       <script type="text/javascript" src="include/javascript/iframe.js"></script>
<!--{/if}-->
这个是iframe的JS功能语句,拥有iframe功能的风格不能掉了.

最后的{eval output();}这个更是重点,记住在制作风格的最后不能漏掉.

以上就是footer.htm的全解说,相比于header.htm来说非常简单,但是有很多重要的变量和条件语句在这里定义,希望大家多多注意一些不能省略的重点地方.