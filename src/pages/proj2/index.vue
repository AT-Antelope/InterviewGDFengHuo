<template>
	<div class="wh-full" style="background-color: whitesmoke">
		<template v-for="item in data.winList" :key="item.id">
			<mWindow :id="item.id" :position="item.position" />
		</template>
	</div>
</template>

<script setup lang="ts">
// 请以pc端方式查看此页面，非以移动视窗标准开发
import { reactive } from "vue";
import mWindow from "/src/pages/proj2/mWindow.vue";

const data = reactive({
	winList: [
		{
			id: "0",
			position: {
				y: 0,
				x: 0,
			},
		},
		{
			id: "1",
			position: {
				y: 0,
				x: 0,
			},
		},
	],
});
</script>

<style lang="scss" scoped></style>
