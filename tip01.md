# 小笔记汇总01

## 1.sprintf 格式化数字

```PHP
sprintf("%03d",$number)//实现1变成001，以此类推
```

## 2.获取被选中下拉框的值

```javascript
$("select").find("option:selected").val()
```

## 3.回到页面的顶部

```javascript
$("html,body").animate(
  {scrollTop:$(.tr_error:frist).offset().top},
  {duration:1000,easing:'easeOutQuint'}
);
```

## 4.JS的两个字符串函数

```PHP
//leetcode例子：请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
//输入：s = "We are happy."
//输出："We%20are%20happy."
return s.replaceAll(' ','%20'); //我想的做法
return s.split(' ').join('%20'); //另一种做法

```

## 5.HTML返回上一页，而保留数据（IE无效）

```PHP
被返回的页面设置 
header('Cache-control: private, must-revalidate'); //支持页面返回
a标签返回上一页
<a href="javascript:history.back(-1)" class="to_index">前のページに戻る</a>
//ie浏览器不支持
```

## 6.表格td自动换行

```CSS
word-break: break-all;
word-wrap: break-word;
```

## 7.PHP当前时间格式化

```PHP
retun date('Y-m-d H:i:s',time());
```

## 8.WinSCP find 文件

![image-20210629102229067](file://C:\Users\mid2067\AppData\Roaming\Typora\typora-user-images\image-20210629102229067.png?lastModify=1626238347)
