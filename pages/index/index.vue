<!-- @author longyoung -->
<template>
	<view class="button-sp-area">
		<button type="primary" plain="true" @click="onScanFace()">活体人脸采集</button>
	</view>
</template>

<script>
	const lyBDFaceAuth = uni.requireNativePlugin('longyoung-BDFaceAuth');
	export default {
		data() {
			return {
				title: ''
			}
		},
		onLoad() {
		},
		methods: {
			onScanFace() {
				console.error("tagg.onScanFace");
				lyBDFaceAuth.scanFace({
					licenseID:"longyoung-face-android",
					actionAry:["Eye", "Mouth", "HeadLeft", "HeadRight", "HeadLeftOrRight", "HeadUp", "HeadDown"]
				}, result => {
					console.log('file://' + result.imgPath);
				});
			},
		}
	}
</script>

<style>
	button {
		margin-top: 30upx;
		margin-bottom: 30upx;
	}

	.button-sp-area {
		margin: 0 auto;
		width: 60%;
	}

	.content {
		text-align: center;
		height: 400upx;
	}

	.wrapper {
		flex-direction: column;
		justify-content: center;
	}

	.button {
		width: 200px;
		margin-top: 30px;
		margin-left: 20px;
		padding-top: 20px;
		padding-bottom: 20px;
		border-width: 2px;
		border-style: solid;
		border-color: #458B00;
		background-color: #458B00;
	}

	.text {
		font-size: 30px;
		color: #666666;
		text-align: center;
	}
</style>
