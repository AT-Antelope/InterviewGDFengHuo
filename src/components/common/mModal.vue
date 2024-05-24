<template>
	<up-modal
		:show="data.show"
		:closeOnClickOverlay="true"
		confirmColor="#F7CF49"
		showCancelButton="true"
		@close="handleClose"
		@confirm="handleComfirm"
		@cancel="handleCancel"
	>
		<slot></slot>
	</up-modal>
</template>

<script setup lang="ts">
import { reactive, watch } from "vue";

const emits = defineEmits(["comfirm", "close"]);
const data = reactive({
	show: false,
});

const handleClose = () => {
	changeShow(false);
};

const handleComfirm = async () => {
	await emits("comfirm");
	handleClose();
};

const handleCancel = () => {
	handleClose();
};

// 改变显示
const changeShow = (show: boolean) => {
	if (typeof show !== "boolean") return (data.show = !data.show);
	data.show = show;
};

watch(
	() => data.show,
	(val) => {
		emits("close", val);
	}
);

defineExpose({
	show: data.show,
	changeShow,
});
</script>

<style lang="scss">
.u-modal__content {
	padding: 0;
}
</style>
