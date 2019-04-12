
---
layout:     post
title:      "PHP版的escape/unescape函数"
subtitle:   ""
date:       2007-03-05 11:16:48
author:     "xxspring"
header-img: "img/header/post_php.jpg"
tags:
    - PHP
---

## PHP版的escape/unescape函数


```
function phpescape($str){
    $sublen=strlen($str);
    $retrunString="";
    for ($i=0;$i<$sublen;$i++){
        if(ord($str[$i])>=127){
        $tmpString=bin2hex(iconv("gb2312","ucs-2",substr($str,$i,2)));
        //$tmpString=substr($tmpString,2,2).substr($tmpString,0,2);window下可能要打开此项
        $retrunString.="%u".$tmpString;
        $i++;
        } else {
            $retrunString.="%".dechex(ord($str[$i]));
        }
    }
    return $retrunString;
}

function unescape($str) {
     $str = rawurldecode($str);
     preg_match_all("/%u.{4}|&#x.{4};|&#d+;|.+/U",$str,$r);
     $ar = $r[0];
     foreach($ar as $k=>$v) {
          if(substr($v,0,2) == "%u")
               $ar[$k] = iconv("UCS-2","GBK",pack("H4",substr($v,-4)));
          elseif(substr($v,0,3) == "&#x")
               $ar[$k] = iconv("UCS-2","GBK",pack("H4",substr($v,3,-1)));
          elseif(substr($v,0,2) == "&#")
               $ar[$k] = iconv("UCS-2","GBK",pack("n",substr($v,2,-1)));
     }
     return join("",$ar);
}
```

<!--下面的程序，只能用于单一汉字-->

php提供的URL编码函数是基于字节的，对由ie的javascript函数escape编码的数据就无能为力了。


```
function escape($str)
{
    preg_match_all("/[\x80-\xff].|[\x01-\x7f]+/",$str,$r);
    $ar = $r[0];
    foreach($ar as $k=>$v)
    {
    if(ord($v[0]) < 128)
        $ar[$k] = rawurlencode($v);
    else
        $ar[$k] = "%u".bin2hex(iconv("GB2312","UCS-2",$v));
    }
    return join("",$ar);
}

function unescape($str)
{
    $str = rawurldecode($str);
    preg_match_all("/(?:%u.{4})|.+/",$str,$r);
    $ar = $r[0];
    foreach($ar as $k=>$v)
    {
        if(substr($v,0,2) == "%u" && strlen($v) == 6)
        $ar[$k] = iconv("UCS-2","GB2312",pack("H4",substr($v,-4)));
    }
    return join("",$ar);
}
```

来至我的网易博客 [原文地址](http://xxspring.blog.163.com/blog/static/114880582007251116480/)
