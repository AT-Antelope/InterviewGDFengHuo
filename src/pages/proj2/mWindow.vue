<template>
	<!-- :draggable="true" -->
	<div
		ref="refWin"
		class="win-wrap"
		:style="{
			top: `${data.position.y}px`,
			left: `${data.position.x}px`,
		}"
		@mousedown.stop="handleFocus"
	>
		<!-- @blur.stop="handleBlur" -->
		<div class="header-wrap" @mousedown="moveStart" @mousemove="moving">
			<!-- draggable -->
			<span>{{ title }}</span>
			<span style="color: white">X</span>
		</div>

		<div class="content-wrap">
			<slot></slot>
		</div>
	</div>
</template>

<script setup lang="ts">
/**
 * 	要求:
 *  1. 点击面板置顶					已解决
		2. 窗口可拖拽						已解决
		3. 视口边缘超出限制			 已解决
		4. 同 id 弹窗位置记忆    已解决
		5. 尺寸调节              ?  简易实现右下角调节，思路:外层包一层div，用flex或grid 在div四周都放宽度为2px的div，单独检测鼠标悬浮、按下，进行拖动处理调节width、height
		6. 尺寸调节限制          已解决
 */

import { computed, onBeforeUnmount, onMounted, reactive, ref } from "vue";
// import { v4 as uuidv4 } from "uuid";

const props = defineProps({
	id: {
		type: String || Number,
	},
	title: {
		type: String,
		default: "新标签",
	},
	position: {
		type: Object,
		default: {
			y: 0,
			x: 0,
		},
	},
});
const refWin = ref();
const data = reactive({
	// id: uuidv4(),
	isMovable: false,
	position: {
		y: 0,
		x: 0,
		// startY: 0,
		// startX: 0,
	},
	isFocusing: false,
	wraper: {
		width: computed(() => refWin?.value?.offsetWidth + 2),
		height: computed(() => refWin?.value?.offsetHeight + 2),
	},
});

// 聚焦 置顶
const handleFocus = () => {
	// 获取所有同类型窗口
	const arrWin = Array.from(document.getElementsByClassName("win-wrap"));
	// 统一清除所有置顶
	arrWin.forEach((item: any) => {
		item.style.zIndex = "0";
	});

	data.isFocusing = true;

	// 置顶
	refWin.value.style.zIndex = 10;
	// console.log("focus", refWin.value.style.zIndex);
};

// 失焦
// const handleBlur = () => {
// 	// if (data.isFocusing) return;

// 	// 取消置顶
// 	refWin.value.style.zIndex = 0;
// 	console.log("blur", refWin.value.style.zIndex);
// };
// document.addEventListener("mousedown", handleBlur);

// 窗口移动 开始
const moveStart = (e: MouseEvent) => {
	// console.log("-------------moveStart-------------");
	// console.log(refWin);
	data.isMovable = true;
	// data.position.startY = e.clientY;
	// data.position.startX = e.clientX;
};

// 窗口移动 移动中
const moving = (e: MouseEvent) => {
	if (!data.isMovable) return;

	// data.position.y += e.clientY - data.position.startY;
	// data.position.x += e.clientX - data.position.startX;

	// !todo 鼠标快速移动会出现位置失准，需改为计算top、left
	requestAnimationFrame(() => {
		data.position.y += e.movementY;
		data.position.x += e.movementX;
	});
};
document.addEventListener("mousemove", moving);

// 检测当前窗口是否超出边界
const checkPosition = (e: MouseEvent) => {
	// 顶部超出
	if (data.position.y < 0) data.position.y = 0;

	// 左侧超出
	if (data.position.x < 0) data.position.x = 0;

	// 底部侧超出
	if (
		data.position.y + refWin.value.offsetHeight >
		refWin.value.parentNode.offsetHeight
	)
		data.position.y =
			refWin.value.parentNode.offsetHeight - refWin.value.offsetHeight;

	// 右侧超出
	if (
		data.position.x + refWin.value.offsetWidth >
		refWin.value.parentNode.offsetWidth
	)
		data.position.x =
			refWin.value.parentNode.offsetWidth - refWin.value.offsetWidth;
};

// 保存当前id位置
const savePosition = () => {
	const winList = JSON.parse(localStorage.getItem("winList") as any) || {};
	winList[props.id as any] = data.position;
	localStorage.setItem("winList", JSON.stringify(winList));
};

// 窗口移动 结束
const moveEnd = (e: MouseEvent) => {
	// 检测当前窗口是否超出边界
	checkPosition(e);

	// 保存当前id位置
	savePosition();

	data.isMovable = false;
	// console.log("-------------moveEnd-------------");
};
document.addEventListener("mouseup", moveEnd);

// 初始化
const init = () => {
	// 获取储存的列表
	const winList = JSON.parse(localStorage.getItem("winList") as any);
	// 获取当前id的下标
	const curIndex = Object.keys(winList).findIndex((key) => key == props.id);
	// 同步位置
	data.position = winList[curIndex];
};

onMounted(() => {
	init();
});

onBeforeUnmount(() => {
	// 解除监听 释放内存
	document.removeEventListener("mousemove", moving);
	document.removeEventListener("mouseup", moveEnd);
	// document.removeEventListener("click", handleBlur);
});
</script>

<style lang="scss" scoped>
.win-wrap {
	position: absolute;
	border: 2px solid white;
	border-radius: 0.5rem;
	overflow: hidden;
	top: 100px;
	left: 100px;
	resize: both;

	.header-wrap {
		height: 2rem;
		line-height: 2rem;
		display: flex;
		justify-content: space-between;
		align-content: center;
		padding: 0 1rem;
		background-color: #3684d7;
		cursor: move;
	}

	.content-wrap {
		// !temp 临时宽高
		min-width: 200px;
		width: 100%;
		min-height: 200px;
		height: 100%;
		border-top: none;
		background-color: #eee5db;
	}
}
</style>
