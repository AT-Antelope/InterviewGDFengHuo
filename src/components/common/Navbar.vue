<template>
	<view class="w-full nav-wrap">
		<template v-for="item in data.navList" :key="item.title">
			<view class="nav-single" @click="changeNav(item)">
				<up-image :src="item.icon" width="40.4rpx" height="40.45rpx"></up-image>

				<text
					:style="{ color: data.curNav == item.title ? '#F7CF49' : '#D8D8D8' }"
				>
					{{ item.title }}
				</text>
			</view>
		</template>
	</view>
</template>

<script setup lang="ts">
import { onMounted, reactive } from "vue";

const props = defineProps({
	cur: {
		type: String,
		default: "首页",
	},
});

const data = reactive({
	curNav: "首页",
	navList: [
		{
			title: "首页",
			icon: "/src/static/img/nav/home.png",
			url: "/pages/index/index",
		},
		{
			title: "我的订单",
			icon: "/src/static/img/nav/order.png",
			url: "/pages/proj2/index",
		},
		{
			title: "个人中心",
			icon: "/src/static/img/nav/my.png",
			url: "/pages/my/index",
		},
	],
});

// todo 转到对应页
const changeNav = (item) => {
	data.curNav = item.title;
	// todo 修改图标激活

	if (!item.url) return;
	uni.navigateTo({
		url: item.url,
	});
};

const init = () => {
	data.curNav = props.cur;
};

onMounted(() => {
	init();
});
</script>

<style lang="scss" scoped>
.nav-wrap {
	position: sticky;
	bottom: 0;
	width: 100%;
	height: 127rpx;
	min-height: 127rpx;
	background: #ffffff;
	box-shadow: 0px -2rpx 6rpx 0px rgba(0, 0, 0, 0.102);

	display: flex;

	.nav-single {
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 10rpx;

		font-size: 20rpx;
		font-weight: normal;
		line-height: 20rpx;
	}
}
</style>
