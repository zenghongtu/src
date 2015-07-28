### 以下是常用的代码收集，没有任何技术含量，只是填坑的积累。转载请注明出处，谢谢。

#### 1. PC - js
- 返回指定范围的随机数(m-n之间)的公式
```javascript
Math.random()*(n-m)+m
```

- [return false](http://stackoverflow.com/questions/1357118/event-preventdefault-vs-return-false)
- [return false](http://www.75team.com/archives/201)
```javascript
// event.preventDefault()会阻挡预设要发生的事件.
// event.stopPropagation()会阻挡发生冒泡事件.
// 而return false则是前面两者的事情他都会做：
// 他会做event.preventDefault();
// 他会做event.stopPropagation();
// 停止callback function的执行并且立即return回来
```

- 两种图片lazy加载的方式
第一个By JS中级交流群 成都-猎巫 第二个By 上海-zenki 
```javascript
// @description 准备为图片预加载使用的插件
// 使用的图片容器css类名为lazy-load-wrap
// 图片真实地址为data-lazy-src
// 当lazy-load-wrap容器进入视口，则开始替换容器内所有需要延迟加载的图片路径，并更改容器的加载状态
//第一种方法
$.fn.compassLazyLoad=function(){
	var _HEIGHT=window.innerHeight,
	_lazyLoadWrap=$('.lazy-load-wrap');

	var methods={
		setOffsetTop:function(){
			$.each(_lazyLoadWrap,function(i,n){
				$(n).attr({
					'top':n.offsetTop-_HEIGHT,
					'status':'wait'
				});
			})
		},
		isShow:function(){
			var _scrollTop=$(window).scrollTop;
			//利用image容器判断是否进入视口，而非image本身
			$.each(_lazyLoadWrap,function(){
				var _that=$(this);
				if (_that.attr('status')==='done') {
					return;
				};
				if (_that.attr('top')<=_scrollTop) {
					_that.find('img[data-lazy-src]').each(function(i,n){
						n.src=$(n).data('lazy-src');
					});
					_that.attr('status','done');
				};
			})
		},
		scroll:function(){
			$(window).on('scroll',function(){
				methods.isShow();
			});
		},
		init:function(){
			methods.setOffsetTop();
			methods.isShow();
			methods.scroll();
		}
	};
	methods.init();

}


//第二种方法

var exist=(function($){
	var timer=null,
	temp=[].slice.call($('.container'));
	ret={};

	for(var i=0,len=temp.length-1;i<=len;i++){
		ret[i]=temp[i];
	}
	var isExist=function(winTop,winEnd){
		for(var i in ret){
			console.log(ret);
			var item=ret[i],
			eleTop=item.offsetTop,
			eleEnd=eleTop+item.offsetHeight;

			if((eleTop>winTop&&eleTop<=winEnd)||(eleEnd>winTop&&eleEnd<=winEnd)){
				$(item).css('background','none');
				new Tab($(item).attr('id'),data).init;
				delete ret[i];
			}
		}
	}
	return {
		timer:timer;
		isExist:isExist;
	};
})($);



//第三种方法
Zepto(function ($) {
    var swiper = new Swiper('.swiper-container', {
        pagination: '.swiper-pagination',
        paginationClickable: true,
        autoplay: 3000,
        loop: true,
        autoplayDisableOnInteraction: false
    });
    (function lazyLoad() {
        var imgs = $(".lazyLoad");
        var src = '';
        $.each(imgs, function (index, item) {
            src = $(item).attr('data-src');
            $(item).attr('src', src);
        });
    })();
});
$(function () {
    var lazyLoadTimerId = null;
    /// 智能加载事件
    $(window).bind("scroll", function () {
        clearTimeout(lazyLoadTimerId);
        lazyLoadTimerId = setTimeout(function () {
            // 延迟加载所有图片
            var isHttp = (location.protocol === "http:");
            $("#ym_images img").each(function () {
                var self = $(this);
                if (self.filter(":above-the-fold").length > 0) {
                    var originUrl = self.attr("data-original");
                    self.attr("src", originUrl);
                }
            });
        }, 500);
    });
});

```

#### 2. Mobile - js


#### 3. [微信 weixin](http://loo2k.com/blog/detecting-wechat-client/)

- UserAgent 判断微信客户端
```javascript
// Mozilla/5.0 (iPhone; CPU iPhone OS 8_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12F70 MicroMessenger/6.1.5 NetType/WIFI
function isWechat() {  
    var ua = navigator.userAgent.toLowerCase();
    return /micromessenger/i.test(ua) || /windows phone/i.test(ua);
}
```

- JS接口安全域名不填写，分享onMenuShareAppMessage直接会取默认值。
```javascript
// 分享onMenuShareAppMessage直接会取默认值
```

- 关闭当前页面
```javascript
WeixinJSBridge.call('closeWindow');
```

- [支付接口方法调用必须在addevent里边调用](http://www.cnblogs.com/true_to_me/p/3565039.html)
```javascript
document.addEventListener('WeixinJSBridgeReady', function onBridgeReady(){
    that.initOrder();
}, false);
```

- 支付接口方法调用必须在
```javascript
WeixinJSBridge.invoke('getBrandWCPayRequest', d, function(res){
    if(res.err_msg == "get_brand_wcpay_request:ok"){
        // alert("支付成功");
        // union.release(d.orderId);
        resetUrl();
        paySuccess('home', d.orderId);
    } else {
        cancelOrder(d.orderId);
        // alert(res.err_msg);
    }
    loading.hide();
});
```