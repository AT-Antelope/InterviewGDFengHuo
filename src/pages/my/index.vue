<template>
	<view class="wh-full page-wrap">
		<view class="header">
			<view class="header-bg"> </view>

			<view class="user-wrap">
				<up-image
					src="/src/static/img/my/userAvatar.png"
					width="147rpx"
					height="152rpx"
					shape="circle"
				></up-image>

				<view class="user-content">
					<text class="user-name">微信用户</text>
					<text class="user-phone">159532387209</text>
				</view>

				<view class="user-more">
					<up-image
						src="/src/static/img/my/qrcode.png"
						width="36rpx"
						height="36rpx"
						@tap="refShare?.changeShow()"
					></up-image>

					<up-image
						src="/src/static/img/common/arrowRight.png"
						width="12rpx"
						height="24rpx"
						style="transform: translateY(-10rpx)"
					></up-image>
				</view>
			</view>
		</view>

		<view class="body-wrap">
			<view class="list-wrap">
				<template v-for="(item, i) in data.opt.list" :key="i">
					<!-- 单个菜单 -->
					<view class="list-single">
						<view class="list-left">
							<up-image
								:src="`/src/static/img/my/list${i}.png`"
								width="72rpx"
								height="78rpx"
							></up-image>

							<text class="list-txt">
								{{ item.label }}
							</text>
						</view>

						<up-image
							src="/src/static/img/common/arrowRight.png"
							width="12rpx"
							height="24rpx"
							style="transform: translateY(-10rpx)"
						></up-image>
					</view>
				</template>
			</view>
		</view>

		<view class="logout-wrap">
			<view class="logout-btn">
				<text class="logout-txt">退出</text>
			</view>
		</view>

		<Navbar cur="个人中心" />
	</view>

	<Share ref="refShare" />
</template>

<script setup>
import { reactive, ref } from "vue";
import Navbar from "/src/components/common/Navbar.vue";
import Share from "/src/pages/my/Share.vue";

const refShare = ref();
const data = reactive({
	opt: {
		list: [
			{
				label: "我的房屋",
			},
			{
				label: "我的报告",
			},
			{
				label: "积分商城",
			},
			{
				label: "我的好友",
			},
			{
				label: "我的评价",
			},
			{
				label: "客服服务",
			},
			{
				label: "建议反馈",
			},
		],
	},
});
</script>

<style lang="scss" scoped>
.page-wrap {
	position: relative;
	display: flex;
	flex-direction: column;

	// 顶部
	.header {
		position: relative;
		width: 100%;
		// height: 477rpx;
		height: 377rpx;
		display: flex;
		// flex-direction: column;
		// justify-content: flex-end;
		align-items: center;
		transform: translate(0, -93rpx);

		.header-bg {
			position: absolute;
			width: 100%;
			height: 377rpx;
			border-radius: 0 0 62rpx 62rpx;
			background-color: #f7cf49;
			z-index: -10;
		}

		.user-wrap {
			width: 100%;
			display: flex;
			align-content: center;
			gap: 20rpx;
			padding: 0 44rpx;

			// 用户信息
			.user-content {
				flex: 1;
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				padding: 26rpx 0;

				.user-name {
					font-size: 38rpx;
					// font-weight: 500;
					font-weight: bold;
					line-height: 38rpx;
					color: #111111;
					// todo 省略超过显示省略号
				}

				.user-phone {
					font-size: 28rpx;
					font-weight: normal;
					line-height: 28rpx;
					color: #666666;
				}
			}

			.user-more {
				display: flex;
				align-items: center;
				gap: 46rpx;
			}
		}
	}

	// 内容区
	.body-wrap {
		flex: 1;
		padding: 0 30rpx;

		.list-wrap {
			display: flex;
			flex-direction: column;
			gap: 55rpx;
			transform: translateY(-100rpx);

			border-radius: 50rpx;
			background: #ffffff;
			box-shadow: 0px 0px 12rpx 0px rgba(0, 0, 0, 0.102);
			padding: 61rpx;

			.list-single {
				display: flex;
				justify-content: space-between;
				align-items: center;

				.list-left {
					display: flex;
					align-items: center;
					gap: 32rpx;
				}

				.list-txt {
					font-size: 30rpx;
					font-weight: 500;
					line-height: 30rpx;
					color: #666666;
				}
			}
		}
	}

	// 退出
	.logout-wrap {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;

		.logout-btn {
			position: absolute;
			bottom: 10rpx;

			border-radius: 32px;
			background: #fffff5;
			padding: 17rpx 61rpx;
		}

		.logout-txt {
			font-size: 30rpx;
			font-weight: normal;
			line-height: 30rpx;
			color: #f7cf49;
		}
	}
}
</style>
