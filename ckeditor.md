# ckeditor的基本使用

## 1.安装

### ckeditor4兼容ie浏览器

1.官网下载即可

2.wordcount下载以及安装

https://github.com/w8tcha/CKEditor-WordCount-Plugin

3.在ckeditor目录下的config.js中加上

```js
config.extraPlugins = 'wordcount,notification';
```

### ckeditor5不兼容ie

## 2.初始化

```html
<textarea name="memo" id="editor1" rows="10" cols="80"></textarea>
```

```js
	    CKEDITOR.replace('editor1', {
            language: 'ja',
	        extraPlugins: ['wordcount','notification'],
	        wordcount: {
	            showWordCount: true, 
                showCharCount: true,
	            maxWordCount: -1,
	            maxCharCount: 2000,
                //以下注释的代码不知道干嘛用的
	            // paragraphsCountGreaterThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationparagraphs").css("background-color", "crimson").css("color", "white").text(currentLength + "/" + maxLength + " - paragraphs").show();
	            // },
	            // wordCountGreaterThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationword").css("background-color", "crimson").css("color", "white").text(currentLength + "/" + maxLength + " - word").show();
	            // },
	            // charCountGreaterThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationchar").css("background-color", "crimson").css("color", "white").text(currentLength + "/" + maxLength + " - char").show();
	            // },
	            // charCountLessThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationchar").css("background-color", "white").css("color", "black").hide();
	            // },
	            // paragraphsCountLessThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationparagraphs").css("background-color", "white").css("color", "black").hide();
	            // },
	            // wordCountLessThanMaxLengthEvent: function (currentLength, maxLength) {
	            //     $("#informationword").css("background-color", "white").css("color", "black").hide();
	            // }
	       }
```

