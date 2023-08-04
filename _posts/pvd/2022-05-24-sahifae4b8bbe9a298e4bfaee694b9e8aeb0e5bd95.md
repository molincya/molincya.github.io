---
id: 2056
title: 自用主题自用修改记录
date: '2022-05-24T14:32:43+08:00'
author: 灵兮
layout: post
guid: 'https://www.coatingrd.com/?p=2056'
permalink: /sahifa%e4%b8%bb%e9%a2%98%e4%bf%ae%e6%94%b9%e8%ae%b0%e5%bd%95/
tie_sidebar_pos:
    - full
tie_post_slider:
    - '1102'
views:
    - '40'
image: /wp-content/uploads/2022/05/易睹1661.png
categories:
    - 木易灵兮
tags:
    - 主题修改
    - 插件
    - 易镀1661
---

## sahifa主题自用修改记录

记录下wordpress sahifa主题修改，免得更新后又要重写。调整了下：段落左右对齐、字间距、首行缩进、超链接颜色及下划线用表格线替代下横线。

## style.css修改

```
<pre class="wp-block-code">```
* {

	margin: 0;
	outline: none;
        line-height: 30px;
	border: 0 none;
}

p {
text-indent: 30px;
padding-bottom:14px;
text-align: justify;
}
p a {
border-bottom: 1px solid; 
}
p a:hover {
	color: red;
}

h1,
h2,
h3,
h4,
h5,
h6 {
    padding-bottom:20px;
}

article h2 {
    border-top: solid 1px #ccc;	
    margin-top:20px;
    padding-top:30px;
}
article img {
    margin-top:10px;
}

```
```

## 无代码开启WordPress文章关键字内链

直接添加Functions.php脚本实现自动Tags内链效果，但是这样的脚本有些问题，比如我们在升级主题或者更换主题的时候还需要重新替换。****TaxoPress****插件可以实现类似功能。

```
<pre class="wp-block-code">```
//自动TAG转内链
$match_num_from = 1; // 一个TAG标签出现几次才加链接
$match_num_to = 1; // 同一个标签加几次链接
add_filter('the_content','tag_link',1);
function tag_sort($a, $b){
if ( $a->name == $b->name ) return 0;
return ( strlen($a->name) > strlen($b->name) ) ? -1 : 1;
}
function tag_link($content){
global $match_num_from,$match_num_to;
$posttags = get_the_tags();
if ($posttags) {
usort($posttags, "tag_sort");
foreach($posttags as $tag) {
$link = get_tag_link($tag->term_id);
$keyword = $tag->name;
$cleankeyword = stripslashes($keyword);
$url = "<a href=\"$link\" title=\"".str_replace('%s',addcslashes($cleankeyword, '$'),__('查看更多 for %s'))."\"";
$url .= ' target="_blank"';
$url .= ">".addcslashes($cleankeyword, '$')."</a>";
$limit = rand($match_num_from,$match_num_to);
$content = preg_replace( '|(<a[^>]+>)(.*)('.$ex_word.')(.*)(</a[^>]*>)|U'.$case, '$1$2%&&&&&%$4$5', $content);
$content = preg_replace( '|(<img)(.*?)('.$ex_word.')(.*?)(>)|U'.$case, '$1$2%&&&&&%$4$5', $content);
$cleankeyword = preg_quote($cleankeyword,'\'');
$regEx = '\'(?!((<.*?)|(<a.*?)))('. $cleankeyword . ')(?!(([^<>]*?)>)|([^>]*?</a>))\'s' . $case;
$content = preg_replace($regEx,$url,$content,$limit);
$content = str_replace( '%&&&&&%', stripslashes($ex_word), $content);
}
}
return $content;
}

```
```

- - - - - -

## Simple Writer主题修改

### Simple Writer修改记录

- 隐藏分类及简介：  
    自定义 ▸ Theme Options Archive Pages Options，Hide Category Title + Category Description from Category Archive Pages
- 调节banner尺寸和文字：  
    自定义 ▸ Theme Options Header Options，设置header都 height和Padding值。

### style.css修改

自定义 ▸ 额外CSS——这种修改不怕主题升级；更换主题失效。

```
<pre class="wp-block-code">```
article h2 {
border-top: dotted 1px #ccc;
margin-top:20px;
padding-top:30px;
}
.entry-content a:hover,.entry-content a:focus,.entry-content a:active{text-decoration:none;color:#ea5029;}
li a{border-bottom:1px dotted #c94f31;}
```
```

### 底部版权信息：

后台编辑的主题Simple Writer文件: 主题页脚 (footer.php) ：

```
<pre class="wp-block-code">```
<a href="https://beian.miit.gov.cn" target="_blank">浙ICP备15043601号-3</a>
```
```

## 简单代码实现WordPress隐藏内容用密码才可显示

添加到当前的主题functions.php页面中

```
<pre class="wp-block-code">```
//隐藏部分内容
function e_secret($atts, $content=null){  
    extract(shortcode_atts(array('key'=>null), $atts));  
    if(isset($_POST['e_secret_key']) && $_POST['e_secret_key']==$key){  
        return '  
<div class="e-secret">'.$content.'</div>  
';  
    }  
    else{  
        return '  
<form action="'.get_permalink().'" method="post" name="e-secret"><p><label for="pwbox-142">输入密码查看加密内容： <input type="password" name="e_secret_key" size="20" /></label> <input type="submit" value="确定" /></p>  
</form>  
';  
    }  
}  
add_shortcode('secret','e_secret'); 
//END
```
```

最后，如果我们需要添加隐藏部分用下面代码包含起来。

```
<pre class="wp-block-code">```
[secret key="输入密码"]隐藏内容[/secret]
```
```

引用：https://www.itbulu.com/wp-hidecode.html