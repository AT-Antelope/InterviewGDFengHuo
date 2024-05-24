<template>
	<view class="wh-full page-wrap">
		<!-- 顶部 -->
		<view class="header">
			<view class="header-bg"> </view>
			<up-image
				src="/src/static/img/index/header.png"
				radius="20rpx"
				width="611rpx"
				height="324rpx"
			></up-image>
		</view>

		<!-- 内容区 -->
		<view class="body-wrap">
			<!-- 大按钮 -->
			<view class="bigbtn-wrap">
				<template v-for="(item, i) in data.opt.bigbtn" :key="i">
					<view class="bigbtn">
						<view class="bigbtn-left">
							<text class="title-text">{{ item.title }}</text>
							<text class="subtitle-text">{{ item.subtitle }}</text>
						</view>

						<up-image
							:src="item.icon"
							width="91.12rpx"
							height="91.12rpx"
						></up-image>
					</view>
				</template>
			</view>

			<!-- 小按钮 -->
			<view class="bitbtn-wrap">
				<view v-for="(item, i) in data.opt.bitbtn" :key="i" class="bitbtn">
					<up-image
						:src="item.icon"
						width="91.12rpx"
						height="91.12rpx"
					></up-image>

					<text class="bitbtn-txt">
						{{ item.title }}
					</text>
				</view>
			</view>

			<!-- 拼团信息 -->
			<view class="list-wrap-outter">
				<view class="list-header">
					<view class="left-bar"></view>
					<text class="list-header-txt">拼团信息</text>
				</view>

				<!-- 列表 -->
				<view class="list-wrap">
					<template v-for="(item, i) in data.list" :key="`list-${item.id}`">
						<view class="list-single" @click="handleGoDetails(item)">
							<up-image
								:src="item.icon"
								width="209rpx"
								height="199rpx"
							></up-image>

							<!-- 单个内容 -->
							<view class="list-content">
								<view class="list-content-top">
									<!-- 顶部标签 -->
									<view class="list-content-top-header">
										<!-- 左侧 拼团 -->
										<view class="list-content-top-header-left">
											<text v-if="item.isEnd"> 已拼{{ item.inGroup }}人 </text>
											<text v-else>
												{{ item.total }}人成团，还差{{
													item.total - item.inGroup
												}}人
											</text>
										</view>

										<!-- 右侧 距结束 -->
										<view class="up-to-over">
											<text class="up-to-over-txt"> 距结束 </text>

											<view class="up-to-over-time-wrap">
												<template v-for="(cur, i) in item.upToOver" :key="i">
													<text v-if="i != 0">:</text>
													<view class="up-to-over-time">
														{{ cur }}
													</view>
												</template>
											</view>
										</view>
									</view>

									<text class="list-title">{{ item.title }}</text>
								</view>

								<view class="list-content-bottom">
									<text class="list-content-bottom-txt"> 拼团价 </text>

									<!-- 当前价格 -->
									<view class="list-content-bottom-price">
										￥<text class="list-content-bottom-price-txt">
											{{ item.price }}</text
										>
									</view>

									<!-- 折扣前价格 -->
									<view class="list-content-bottom-priceRaw">
										￥<text class="list-content-bottom-priceRaw-txt">
											{{ item.priceRaw }}</text
										>
									</view>
								</view>
							</view>
						</view>

						<!-- 分割线 -->
						<up-line v-if="i < data.list.length - 1"></up-line>
					</template>
				</view>
			</view>
		</view>

		<Navbar />
	</view>
</template>

<script setup lang="ts">
import { reactive } from "vue";
// !todo 暂不配@符
import Navbar from "/src/components/common/Navbar.vue";

const data = reactive({
	list: [
		{
			id: 0,
			icon: "/src/static/img/index/iconAvatar.png",
			isEnd: false,
			inGroup: 2,
			total: 3,
			title: "广州市保利花园1单元16楼1604间",
			price: "489.00",
			priceRaw: "589.00",
			// 暂使用静态代替
			upToOver: [12, 23, 10],
		},
		{
			id: 1,
			icon: "/src/static/img/index/iconAvatar.png",
			isEnd: true,
			inGroup: 2,
			total: 3,
			title: "广州市保利花园1单元16楼1604间",
			price: "489.00",
			priceRaw: "589.00",
			// 暂使用静态代替
			upToOver: [12, 23, 10],
		},
	],
	opt: {
		bigbtn: [
			{
				title: "快速验房",
				subtitle: "即刻下单",
				icon: "/src/static/img/index/iconBigBtn0.png",
			},
			{
				title: "装修监理",
				subtitle: "即刻下单",
				icon: "/src/static/img/index/iconBigBtn1.png",
			},
		],
		bitbtn: [
			{
				title: "平台优势",
				icon: "/src/static/img/index/bitbtn0.png",
			},
			{
				title: "服务内容",
				icon: "/src/static/img/index/bitbtn1.png",
			},
			{
				title: "服务流程",
				icon: "/src/static/img/index/bitbtn2.png",
			},
			{
				title: "经典案例",
				icon: "/src/static/img/index/bitbtn3.png",
			},
		],
	},
});

// 转到详情页
const handleGoDetails = (item) => {
	console.log(item);
	uni.navigateTo({
		url: `/pages/houseDetails/index?id=${item.id}`,
		fail: (err) => {
			console.log(err);
		},
	});
};
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
		flex-direction: column;
		justify-content: flex-end;
		align-items: center;

		.header-bg {
			position: absolute;
			width: 100%;
			height: 377rpx;
			border-radius: 0 0 62rpx 62rpx;
			background-color: #f7cf49;
			transform: translate(0, -93rpx);
			z-index: -10;
		}
	}

	// 内容区
	.body-wrap {
		flex: 1;
		padding: 0 30rpx;

		.bigbtn-wrap {
			display: flex;
			gap: 22rpx;
			margin-top: 37rpx;

			// 大按钮
			.bigbtn {
				width: 100%;
				// height: 149rpx;
				size: 36rpx;
				padding: 30rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				border-radius: 20rpx;
				background: linear-gradient(180deg, #ffc501 0%, #f7cf49 100%);
				box-shadow: 0px 0px 12rpx 0px #fdc712;

				.bigbtn-left {
					height: 100%;
					display: flex;
					flex-direction: column;
					justify-content: space-between;

					color: #ffffff;

					.title-text {
						font-size: 36rpx;
						font-weight: 500;
						line-height: 36rpx;
						letter-spacing: 0em;
					}

					.subtitle-text {
						font-size: 22rpx;
						font-weight: normal;
						line-height: 22rpx;
						letter-spacing: 0em;
					}
				}
			}
		}

		// 小按钮
		.bitbtn-wrap {
			margin: 40rpx 0;
			display: grid;
			grid-template-columns: repeat(4, minmax(0, 1fr));

			.bitbtn {
				display: flex;
				flex-direction: column;
				align-items: center;
				gap: 28rpx;

				.bitbtn-txt {
					font-size: 20rpx;
					font-weight: normal;
					line-height: 20rpx;
					letter-spacing: 0em;
				}
			}
		}

		// 列表
		.list-wrap-outter {
			.list-header {
				display: flex;
				align-items: center;
				gap: 19rpx;

				.left-bar {
					width: 9rpx;
					height: 49rpx;
					border-radius: 5rpx;
					background-color: #f7cf49;
				}

				.list-header-txt {
					width: 120rpx;
					height: 30rpx;
					font-size: 30rpx;
					// font-weight: normal;
					font-weight: bold;
					line-height: 30rpx;
					color: #111111;
				}
			}

			// 列表
			.list-wrap {
				display: flex;
				flex-direction: column;

				.list-single {
					display: grid;
					grid-template-columns: 209rpx minmax(0, 1fr);

					padding: 20rpx 0;
					gap: 20rpx;

					.list-content {
						display: flex;
						flex-direction: column;
						justify-content: space-between;

						.list-content-top {
							display: flex;
							flex-direction: column;
							justify-content: space-between;
							gap: 20rpx;

							.list-content-top-header {
								display: flex;
								justify-content: space-between;

								.list-content-top-header-left {
									display: flex;
									align-items: center;

									padding: 8rpx 10rpx;
									border-radius: 10rpx;
									background: linear-gradient(90deg, #ffd9ae 0%, #ffb300 93%);

									font-size: 20rpx;
									color: #ffffff;
									line-height: 20rpx;
								}

								.up-to-over {
									display: flex;
									align-items: center;

									.up-to-over-txt {
										margin-right: 10rpx;

										font-size: 24rpx;
										// font-weight: normal;
										font-weight: bold;
										line-height: 24rpx;
										letter-spacing: 0em;
										color: #111111;
									}

									// 时间 包围
									.up-to-over-time-wrap {
										display: flex;
										align-items: center;
										gap: 2rpx;

										.up-to-over-time {
											display: flex;
											justify-content: center;
											align-items: center;

											// width: 33rpx;
											width: 37rpx;
											height: 37rpx;
											border-radius: 10rpx;
											background: #373737;

											// font-size: 22rpx;
											font-size: 18rpx;
											font-weight: normal;
											line-height: 22rpx;
											color: #ffffff;
										}
									}
								}
							}

							.list-title {
								font-size: 28rpx;
								// font-weight: 500;
								font-weight: bold;
								line-height: 28rpx;
								color: #373737;
							}
						}

						.list-content-bottom {
							display: flex;
							align-items: center;
							// gap: 4rpx;
							gap: 8rpx;

							.list-content-bottom-txt {
								font-size: 24rpx;
								line-height: 24rpx;
								color: #f7cf49;
							}

							.list-content-bottom-price {
								font-weight: 400;
								font-size: 24rpx;
								color: #fe7036;

								.list-content-bottom-price-txt {
									font-size: 32rpx;
									font-weight: 500;
									line-height: 32rpx;
								}
							}

							.list-content-bottom-priceRaw {
								font-weight: 400;
								font-size: 22rpx;
								color: #cccccc;
								// transform: scale(.5);

								.list-content-bottom-priceRaw-txt {
									font-weight: 400;
									font-size: 26rpx;
								}
							}
						}
					}
				}
			}
		}
	}
}
</style>
