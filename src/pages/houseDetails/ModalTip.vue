<template>
	<mModal ref="refModal" @comfirm="handleComfirm">
		<text class="txt-tip"> 您还没有添加房屋信息，赶快去添加一个吧 </text>
	</mModal>
</template>

<script setup>
import { ref } from "vue";
import mModal from "/src/components/common/mModal.vue";

const emits = defineEmits(["comfirm"]);
const refModal = ref();

const handleComfirm = async () => {
	await emits("comfirm");
	refModal.value.changeShow();
};

const changeShow = () => {
	refModal.value.changeShow();
};

defineExpose({
	changeShow,
});
</script>

<style lang="scss" scoped>
.txt-tip {
	font-size: 30rpx;
	font-weight: 500;
	line-height: 30rpx;
	color: #111111;
}
</style>
