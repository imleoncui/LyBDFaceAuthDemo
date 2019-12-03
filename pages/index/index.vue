<!-- @author longyoung http://www.html5plus.org/doc/zh_cn/nativeobj.html-->
<template>
	<view class="button-sp-area">
		<button type="primary" plain="true" @click="onScanFace()">活体人脸采集</button>
	</view>
</template>

<script>
	import permijs from '../../utiles/permission.js'
	const lyBDFaceAuth = uni.requireNativePlugin('longyoung-BDFaceAuth');
	export default {
		data() {
			return {
				title: ''
			}
		},
		onLoad() {
			//权限
			// #ifdef APP-PLUS
			if (uni.getSystemInfoSync().platform == "ios") {
				// this.judgeIosPermission('camera');//相机
			} else if (uni.getSystemInfoSync().platform == "android") {
				this.requestAndroidAPermission('android.permission.CAMERA'); //相机
				// this.requestAndroidPermission('android.permission.READ_EXTERNAL_STORAGE');//外部存储(含相册)读取权限
				// this.requestAndroidPermission('android.permission.WRITE_EXTERNAL_STORAGE');//外部存储(含相册)写入权限
			}
			// #endif
		},
		methods: {
			onScanFace() {
				console.error("tagg.onScanFace");
				lyBDFaceAuth.scanFace({
					licenseID: "longyoung-face-android",
					actionAry:["Eye", "Mouth", "HeadLeft", "HeadRight", "HeadLeftOrRight", "HeadUp", "HeadDown"],//不传无动作
					isLivenessRandom: 0, //不传默认有序，0有序，1随机
					isSound: 1, //不传默认有声音，0无声，1有声
				}, result => {
					console.log('file://' + result.imgPath);
					
					//图片上传服务器
					uni.uploadFile({
						url:'http://api.longyoung.com/api/open/common/uploadImgTemp',//图片上传地址
						filePath: 'file://' + result.imgPath,//图片本地路径，上传服务器需要加这个头'file://'
						method: 'post',
						name: 'imgFile',//上传图片参数名
						success: (res) => {
							var data = res.data;
						}
					});

					//***有些同学，后台强烈要求传base64，下面是图片转base64的方法，没此需求的可以无视。
					var bitmapT = new plus.nativeObj.Bitmap("test"); //test标识谁便取
					// 从本地加载Bitmap图片
					bitmapT.load(result.imgPath, function() {
						console.log('加载图片成功');
						var base4 = bitmapT.toBase64Data();
						console.log('lygg.base64=' + base4);
					}, function(e) {
						console.log('加载图片失败：' + JSON.stringify(e));
					});
					//***有些同学，后台强烈要求传base64，下面是图片转base64的方法，没此需求的可以无视。

				});
			},
			//权限
			async requestAndroidPermission(permisionID) {
				var result = await permijs.requestAndroidPermission(permisionID);
				var strStatus;
				if (result == 1) {
					strStatus = "已获得授权";
				} else if (result == 0) {
					strStatus = "未获得授权";
					uni.showToast({
						title: "请打开权限，否则无法使用",
						icon: 'none'
					});
				} else {
					strStatus = "被永久拒绝权限";
					permijs.gotoAppPermissionSetting();
					uni.showToast({
						title: '请打开权限，否则无法使用',
						icon: 'none'
					});
				}
				console.log("lygg.strStatus=" + strStatus + ",result=" + result);
			},
			judgeIosPermission: function(permisionID) {
				var result = permijs.judgeIosPermission(permisionID)
				if (result) {} else {
					permijs.gotoAppPermissionSetting();
					uni.showToast({
						title: '请打开权限，否则无法使用',
						icon: 'none'
					});
				}
				console.log("lygg.result=" + result);
			},
		}
	}
</script>

<style>
	button {
		margin-top: 30rpx;
		margin-bottom: 30rpx;
	}

	.button-sp-area {
		margin: 0 auto;
		width: 60%;
	}
</style>
