# 微信分享及支付

根据网上同名插件修改而成。

安装：

	cordova plugin add {path}/com.justep.cordova.plugin.weixin.v3 --variable WEIXIN_APPID=wx394d8a19e8260xxx --variable WEIXIN_PARTNER_ID=XXX --variable WEIXIN_API_KEY=XXX

参数：

- WEIXIN_APPID: 在微信开发平台为APP申请的appKey (https://open.weixin.qq.com)
- WEIXIN_PARTNER_ID: 微信商户ID。微信支付使用。如果没有开通支付功能，此值随便填。
- WEIXIN_API_KEY: 微信商户API KEY。如果需要调用生成prepayId的接口，则此参数必填。如果prepayId由后台生成，无需app自行计算，则该值随便填。

微信分享：

	var weixin = navigator.weixin;
	if (weixin == null)
	{
		console.log("*** cannot use weixin");
		return;
	}

	var scene = weixin.Scene.SESSION; // 分享到朋友圈。分享到好友用: weixin.Scene.TIMELINE;

	weixin.share({
		message: {
			title: "上靠谱网，寻根问祖，修缮家谱", 
			description: "我在靠谱网上修家谱，邀请你和我一起来协作。",
			mediaTagName: "daca",
			thumb: "http://dacatec.com/jiapu/m2/icon96.png",
			media: {
				type: weixin.Type.WEBPAGE,   // webpage
				webpageUrl: "http://a.app.qq.com/o/simple.jsp?pkgname=com.daca.kaopu"    // webpage
			}
		},
		scene: scene,
	}, function () {
		//alert("Success");
	}, function (reason) {
		// 当reason为空时，为用户取消分享操作。
		if (reason)
			alert("Failed: " + reason);
	});

关于支付，一般建议由后端服务调用prepay生成prepayId传给APP，再由APP调用 sendPayReq 接口。

接口参考：www/weixin.js

## bugs

- 用户取消分享时，安卓平台会当作成功处理。

