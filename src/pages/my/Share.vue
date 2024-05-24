<template>
	<up-overlay :show="data.show" :opacity="0.85">
		<view class="share-warp-outter" @tap.stop="data.show = false">
			<view class="share-header">
				<text class="share-title"> 邀请好友 </text>
				<text class="share-subtitle"> 你与好友都可获得66积分 </text>
			</view>

			<!-- 二维码 -->
			<view id="content-wrap" class="content-wrap">
				<up-image
					src="/src/static/img/my/avatarShare.png"
					width="172rpx"
					height="172rpx"
					style="
						border-radius: 99px;
						border: 4rpx solid white;
						position: absolute;
						top: 100rpx;
						z-index: 30;
					"
				></up-image>

				<view class="content" @tap.stop>
					<text class="content-title"> 微信用户 </text>
					<text class="content-subtitle">
						Hi 朋友，我在小工蜂，快速验房/装修，一起来呀
					</text>

					<view class="qrcode-wrap">
						<up-image
							src="/src/static/img/my/qrcodeShare.png"
							width="260rpx"
							height="260rpx"
						></up-image>
					</view>

					<up-image
						src="/src/static/img/common/logo.png"
						width="255rpx"
						height="113rpx"
						style="margin-top: 36rpx 0 40rpx"
					></up-image>
				</view>
			</view>

			<!-- 底部按钮 -->
			<view class="footer-wrap" @tap.stop>
				<view class="btn-save" @tap.stop="saveQrcode"> 保存图片 </view>
				<view class="btn-invite"> 马上邀请好友 </view>
			</view>
		</view>
	</up-overlay>
</template>

<script setup>
import { reactive } from "vue";
import html2canvas from "html2canvas";
import dayjs from "dayjs";

const data = reactive({
	show: false,
});

// 保存二维码
const saveQrcode = async () => {
	const curElement = document.getElementById("content-wrap");

	const canvas = await html2canvas(curElement);
	const image = canvas.toDataURL("image/png");

	// 以下代码用于下载生成的图片
	const link = document.createElement("a");
	link.download = `share_${dayjs().format("YYYY-MM-DD")}.png`;
	link.href = image;
	link.click();
};

const changeShow = () => {
	console.log(data.show);
	data.show = !data.show;
};

defineExpose({
	changeShow,
});
</script>

<style lang="scss" scoped>
.share-warp-outter {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	padding: 110rpx 0;

	.share-header {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 22rpx;

		.share-title {
			font-size: 42rpx;
			font-weight: 500;
			line-height: 42rpx;
			color: #f7cf49;
		}

		.share-subtitle {
			font-size: 26rpx;
			font-weight: normal;
			line-height: 26rpx;
			color: #ffffff;
		}
	}

	// 内容区
	.content-wrap {
		position: relative;
		flex: 1;
		// width: 690px;
		width: 100%;
		padding: 0 30rpx;
		// height: 857px;
		border-radius: 20px;

		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.content {
			width: 100%;
			display: flex;
			flex-direction: column;
			align-items: center;
			background: #ffffff;
			border-radius: 20px;

			.content-title {
				margin-top: 141rpx;

				font-size: 38rpx;
				font-weight: 500;
				line-height: 38rpx;
				color: #111111;
			}
			.content-subtitle {
				margin-top: 49rpx;

				font-size: 26rpx;
				font-weight: normal;
				line-height: 26rpx;
				color: #111111;
			}

			.qrcode-wrap {
				margin-top: 67rpx;
				width: 347rpx;
				height: 347rpx;
				display: flex;
				justify-content: center;
				align-items: center;

				background-image: url("/src/static/img/my/qrcodeOutter.png");
				background-size: 100% 100%;
				background-repeat: no-repeat;
			}
		}
	}

	// 底部按钮
	.footer-wrap {
		display: flex;
		justify-content: space-between;
		gap: 37rpx;

		font-size: 30rpx;
		font-weight: normal;
		line-height: 30rpx;
		color: #ffffff;

		> * {
			border-radius: 999px;
		}

		.btn-save {
			padding: 32rpx 81rpx;
			background: #393939;
		}

		.btn-invite {
			padding: 32rpx 96rpx;
			background: #f7cf49;
		}
	}
}
</style>
