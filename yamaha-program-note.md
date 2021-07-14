# YAMAHA中国官网项目总结

## p限制行数显示，超出部分省略号

```css
    display: -webkit-box; //使用line-clamp必带，不然会失效
    -webkit-box-orient: vertical;//同上
    -webkit-line-clamp: 2; 
    overflow: hidden;/*内容超出后隐藏*/
    text-overflow: ellipsis;/* 超出内容显示为省略号*/
```

## 文字垂直居中

在不知道具体行高的情况下，可以用伪元素进行文字的垂直居中

```css
.activity::before{
    display: inline-block;
    content: "";
    height: 100%;
    vertical-align: middle;
}
```

## 修改bootstrap.js中的源码实现模态框居中

```js
Modal.prototype.adjustDialog = function () {
    var modalIsOverflowing = this.$element[0].scrollHeight > document.documentElement.clientHeight

    this.$element.css({
      paddingLeft:  !this.bodyIsOverflowing && modalIsOverflowing ? this.scrollbarWidth : '',
      paddingRight: this.bodyIsOverflowing && !modalIsOverflowing ? this.scrollbarWidth : ''
    })
    // 是弹出框居中。。。
    var $modal_dialog = $(this.$element[0]).find('.modal-dialog');
    var m_top = ( $(window).height() - $modal_dialog.height() )/2;
    $modal_dialog.css({'margin': m_top + 'px auto'});
  }
```

## Vedio标签

```html
        <div class="modal-content video-box">
          <video id="video" width="100%" height="100%" muted controls controlslist="nodownload" disablePictureInPicture>
              <source src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4" type="video/mp4">
          </video>
          <div class="video-img"></div>
        </div>
```

muted是静音

controls是加载控件

controlslist=“nodownload”//把下载按钮隐藏

disablePictureInPicture //把画中画隐藏

下面的div class “video-img”，是播放视频的时候，会有一个播放按钮

```css
/* 视频播放 start */
.video-box{
	position: relative;
}

.video-box video{
	display: inline-block;
    vertical-align: baseline;
}

.video-box .video-img{
	position: absolute;
    top: 100px;
    bottom: 0;
    width: 100%;
    z-index: 999;
    background: url(/images/icon/play.svg) no-repeat;
    background-size: 99% 47%;
	cursor:pointer
}
/* 视频播放 end */
```



```js
  $(".video-img").click(function(){
	    $(this).hide();
	    $("#video")[0].play();
	});
```

## 图片根据宽高自动剪切1:1显示

```css
    object-fit:cover;
```

##  绝对定位元素的水平垂直居中

```css
    left: 50%;
    bottom: 50%;
    transform: translate(-50%, -50%);
    max-width: 50%;
    text-align: center;
```

## bootstrap下拉框悬浮

```css
 .dropdown:hover > .dropdown-menu {
     display: block;
 }

 .dropdown>.dropdown-toggle:active {
    pointer-events: none;
     
```

## 使图片颜色反转

```css
-webkit-filter: invert(1);
filter: invert(1);
```

