<template>
	<view class="wh-full page-wrap-outter">
		<!-- 顶部标签 -->
		<view class="tag-top-wrap">
			<view class="tag-top">
				<text style="color: #fd5e31"> * </text>
				<text class="tag-top-txt">选择房屋</text>
			</view>
		</view>

		<!-- 内容区 -->
		<view class="content-wrap-outter">
			<view class="address-wrap">
				<!-- 图标 -->
				<up-image
					src="/src/static/img/houseDetails/home.png"
					width="30.66rpx"
					height="24.81rpx"
				></up-image>

				<view class="address">
					<text class="address-1"> 广东省广州市黄埔区科学大道 </text>
					<text class="address-2"> 保利花园1单元16楼1604间 </text>

					<view class="address-tag-wrap">
						<template v-for="item in data.address.tags" :key="item">
							<view class="address-tag">
								{{ item }}
							</view>
						</template>
					</view>
				</view>

				<!-- 箭头 -->
				<up-image
					src="/src/static/img/common/arrowRight.png"
					width="12rpx"
					height="24rpx"
				></up-image>
			</view>

			<!-- !attention 暂用此方法简易实现，复杂情况需替换 -->
			<view class="form-wrap-outter">
				<!-- 建筑面积 -->
				<text class="form-required"> * </text>
				<text class="form-title">建筑面积:</text>
				<up-input placeholder="请填写" border="surround" clearable>
					<template #suffix>
						<text class="txt-mark">平方米（m²）</text>
					</template></up-input
				>

				<!-- 请选择验房时间 -->
				<text class="form-required"> * </text>
				<text class="form-title">请选择验房时间:</text>
				<up-input
					placeholder="请填写"
					border="surround"
					clearable
					@tap.stop.prevent="selectDate"
				>
					<template #suffix>
						<view class="time-range-suffix">
							<!-- <up-line direction="col" color="707070"></up-line> -->

							<up-image
								src="/src/static/img/houseDetails/calc.png"
								width="30.56rpx"
								height="30.56rpx"
							></up-image>
						</view> </template
				></up-input>
			</view>

			<!-- 模态框 验房时间 -->
			<ModalCalendar ref="refModalCalendar" />
		</view>

		<view class="footer-wrap">
			<view class="footer footer-group">
				<view>
					<text class="footer-price">￥</text>
					<text class="footer-price-txt">00:00</text>
				</view>
				<text class="footer-txt">发起拼团</text>
			</view>

			<view class="footer footer-order">
				<view>
					<text class="footer-price">￥</text>
					<text class="footer-price-txt">00:00</text>
				</view>
				<text class="footer-txt">下单支付</text>
			</view>
		</view>
	</view>

	<ModalTip ref="refModalTip" @comfirm="handleTipComfirm" />
</template>

<script setup lang="ts">
import { onMounted, reactive, ref } from "vue";
import ModalCalendar from "/src/pages/houseDetails/ModalCalendar.vue";
import ModalTip from "/src/pages/houseDetails/ModalTip.vue";

// 模态框 时间选择
const refModalCalendar = ref();
// 模态框 无房屋信息提示
const refModalTip = ref();
const data = reactive({
	address: {
		tags: ["住宅", "160m2", "四房二厅"],
	},
	opt: {},
});

const selectDate = () => {
	refModalCalendar.value.changeShow();
};

// 无房屋提示 确认
const handleTipComfirm = () => {
	// todo 判断逻辑处理
	console.log("无房屋提示 确认");
};

onMounted(() => {
	refModalTip.value.changeShow();
});
</script>

<style lang="scss" scoped>
.page-wrap-outter {
	position: relative;
	height: 100%;
	display: flex;
	flex-direction: column;
	background-color: #f9f9f9;
	box-shadow: inset 0px 3rpx 12rpx 0px rgba(0, 0, 0, 0.102);

	// 顶部标签
	.tag-top-wrap {
		padding: 38.41rpx 0;

		.tag-top {
			width: 211.23rpx;
			height: 65rpx;

			display: flex;
			justify-content: center;
			align-items: center;
			gap: 10rpx;

			border-radius: 0 99px 99px 0;
			background: #ffffff;
			box-shadow: 0px 0px 30px 0px rgba(0, 0, 0, 0.102);

			font-size: 28rpx;
			font-weight: normal;
			line-height: 28rpx;

			.tag-top-txt {
				font-size: 30rpx;
				font-weight: 500;
				line-height: 30rpx;
				color: #111111;
			}
		}
	}

	// 内容区
	.content-wrap-outter {
		flex: 1;
		border-radius: 20rpx;
		padding: 0 30rpx;

		.address-wrap {
			display: grid;
			grid-template-columns: 40rpx minmax(0, 1fr) 40rpx;
			gap: 10rpx;

			padding: 28rpx 27rpx;
			background: #ffffff;

			.address {
				display: flex;
				flex-direction: column;
				padding-top: 12rpx;

				.address-1 {
					font-size: 24rpx;
					// font-weight: normal;
					font-weight: bold;
					line-height: 24rpx;
					color: #333333;
				}

				.address-2 {
					margin-top: 16rpx;

					font-size: 32rpx;
					// font-weight: 500;
					font-weight: bold;
					line-height: 32rpx;
					color: #373737;
				}

				.address-tag-wrap {
					margin: 18rpx 0;
					display: flex;
					align-items: center;
					gap: 14rpx;

					.address-tag {
						padding: 7rpx 10rpx;
						border-radius: 10rpx;
						background: #f7cf49;

						font-size: 22rpx;
						font-weight: normal;
						line-height: 22rpx;
						color: #ffffff;

						text-align: center;
					}
				}
			}
		}

		.form-wrap-outter {
			margin-top: 48rpx;
			display: grid;
			grid-template-columns: 13rpx 32% minmax(0, 415rpx);
			gap: 17rpx;

			.form-required {
				color: #fd5e31;
				display: flex;
				align-items: center;
				margin-top: 10rpx;
			}

			.form-title {
				font-size: 28rpx;
				font-weight: normal;
				line-height: 28rpx;
				color: #999999;

				align-self: center;
			}

			.txt-mark {
				font-size: 22rpx;
				font-weight: normal;
				line-height: 22rpx;
				color: #333333;
			}

			.time-range-suffix {
				width: 76rpx;
				// height: 76rpx;
				height: 44rpx;
				border-left: 2rpx solid #10707070;
				display: flex;
				justify-content: center;
				align-items: center;
			}
		}
	}

	.footer-wrap {
		width: 100%;
		// bottom: 0;
		display: flex;
		justify-content: space-between;
		gap: 70rpx;
		padding: 0 30rpx 38rpx;

		.footer {
			display: flex;
			flex-direction: column;
			gap: 6rpx;
			padding: 17rpx 95rpx;
			border-radius: 999rpx;

			font-size: 36rpx;
			font-weight: normal;
			line-height: 36rpx;
			color: #333333;

			.footer-price {
				font-weight: 400;
				font-size: 30rpx;
			}

			.footer-price-txt {
				font-weight: 400;
				font-size: 36rpx;
			}

			.footer-txt {
				font-size: 30rpx;
				// font-weight: 500;
				font-weight: bold;
				line-height: 30rpx;
			}
		}

		.footer-group {
			background: linear-gradient(180deg, #f7e29f 0%, #eecc60 100%);
		}

		.footer-order {
			background: linear-gradient(180deg, #fe7c3a 0%, #ff4c2b 100%);
			color: #ffffff;
		}
	}
}
</style>
