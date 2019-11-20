# LyBDFaceAuth
uniapp安卓端百度人脸识别、活体检测、人脸采集demo

#### <u>百度官方资料准备</u>

1.详细步骤请查看[官方文档](https://ai.baidu.com/docs#/FaceSDK-Collect-WithLiveness-Android/top)。

2.需要准备License授权文件，文件名idl-license.face-android。

#### <u>接入步骤</u>


1.在根目录创建文件夹nativeplugins，下载插件longyoung-BDFaceAuth，放进这个目录。

2.将百度授权文件License放到 nativeplugins/longyoung-BDFaceAuth/android/assets/idl-license.face-android

3.manifest.json文件，选中「App原生插件配置」，选中本地插件，选择使用插件longyoung-BDFaceAuth。

4.调用插件，需要传入licenseID，代码如下：

```
<script>
	const lyBDFaceAuth = uni.requireNativePlugin('longyoung-BDFaceAuth');
	export default {
		data() {
			return {
				title: ''
			}
		},
		methods: {
			onScanFace() {
				lyBDFaceAuth.scanFace({licenseID:"longyoung-face-android"}, result => {
					console.log('file://' + result.imgPath);
				});
			},
		}
	}
</script>
```

#### <u>注意事项</u>

图片存到本地缓存目录，/storage/emulated/0/Android/data/com.longyoung.facedemo/cache/faceImg/face1573781316334.png，上传服务器需要加这个头'file://'