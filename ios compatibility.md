# ios下的问题
做的移动端页面和响应式页面时遇到的兼容性问题：
### 1. audio

audio在ios上的兼容性问题如下：

 音频的开始和停止在**safari**下都需要手动操作。标签设置的preload属性会被忽略，导致不能自动播放；网页关闭后，音频仍然播放，不会随网页的关闭而停止，除非把浏览器彻底关掉（使用微信浏览器则退出微信）。
 	
 解决方法：
 	
 自动播放：
 		
 		//player为audio元素
 		player.load();//ios9及以上需要先load再play
 		player.play();//ios9以下不需要load
 		
 	 
 **（关于load的说法，因为没有ios9以下的设备进行验证load的必须性，所以此项仅供参考。不过ios9及以上设备使用load是没有问题的) **
 	 
 自动停止：
 	
 		window.onunload = stopVideo;
 		//在关闭网页的时候停止音频播放。stopVideo是自己实现的函数。
 		
 ** 有人提供了另一种思路，通过document.hidden来判断网页是否缩小、被切换、关闭，然后将音频停止。不过有一个问题，难道浏览器要一直轮询hidden状态吗？如果不的话，怎么得知网页状态的变化呢？或许应该好好看看js高程 **
 		
 	

 		
### 2. position:sticky
 ios的浏览器，多数在滚动的时候会**暂停所有js代码执行**，注意，是** 所有的 **，包括你提前设定好的定时器。
 
 有一个比较直观的例子，你在ios的微信中点开公众号，查看gif动图，当在屏幕在滚动的过程中时，gif会定格在某一画面，等你松手之后gif才会继续播放。
 
 js就像gif一样，在屏幕滚动的时候会被暂停，滚动结束后才继续。
 
 遇到的问题：如何在ios端实现粘性布局？
 
 解决方案： position:sticky。 这个css3新属性几乎是为ios特别定制的，它很好的解决了ios的粘性布局问题。

 position:sticky的表现上像是position:fixed和position:relative的结合体，设置了position:sticky的元素，特征如下：

     - 元素不会脱离文档流，并保留元素在文档流中占位的大小
     - 元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置
     - 元素固定的相对偏移是相对于离它最近的具有滚动框的祖先元素，如果祖先元素都不可以滚动，那么是相对于viewport来计算元素的偏移

 sticky其生效条件已经有人研究过，我就不再赘述了。
 
PS：也许会有其他库来实现粘性布局，不过如果只实现粘性布局的话，就没必要引入库了，自己写写js，ios使用sticky属性，非ios增加对scroll的监听，通过计算位置来调整positon。
 
##### 参考链接：
[position:sticky实现iOS6+下的粘性布局](http://efe.baidu.com/blog/position-sticky/)


# ie的问题

### ie8下<b>跨域</b>传送get请求(使用的1.10的jquery不兼容,只得用原生的来写)

    if(!!window.XDomainRequest){
        var xdr = new XDomainRequest();
        xdr.open("GET","http://api.smartstudy.com/user/captcha/phone/check?"+phone+"&captchaPhone="+phoneCaptcha);
        xdr.send();
        xdr.onload=function () {
          console.log(xdr.responseText);//responseText 是字符串,如果要提取其中属性的话需要进行处理
        }
    }

