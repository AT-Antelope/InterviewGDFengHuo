<template>
	<mModal ref="refModal" @close="handleShowChange">
		<view
			class="modal-wrap"
			:style="{ width: data.curTab != '日期' ? '100%' : '' }"
		>
			<view class="tab-wrap">
				<template v-for="item in data.opt.tabList" :key="item">
					<view
						class="tab-single"
						:style="{
							color: data.curTab == item ? '#333333' : '#CCCCCC',
							'background-color': data.curTab == item ? '#E5E5E5' : '',
						}"
						@click="changeTab(item)"
					>
						{{ item }}
					</view>
				</template>
			</view>

			<view v-if="data.curTab == '日期'" class="calendar-wrap">
				<uni-calendar
					:insert="true"
					:start-date="'2024-5-1'"
					:end-date="oDay.format('YYYY-MM-DD')"
					@change="changeDate"
				/>
			</view>

			<view v-else class="time-range-wrap-outter">
				<template v-for="(item, i) in data.timeRange" :key="item.val">
					<up-line v-if="i != 0"></up-line>

					<view class="time-range-wrap" @tap="selectTimeRnage(item)">
						<text
							:class="
								verifyExpired(item) ? 'expired-time' : 'expired-time-normal'
							"
							>{{ item.from }}-{{ item.to }}</text
						>

						<text v-if="verifyExpired(item)" class="expired">已过期</text>
						<view v-else>
							<view v-if="data.curTimeRange == item.val" class="checked">
								<view style="position: absolute; top: -10rpx">
									<up-image
										src="/src/static/img/common/check.png"
										width="22rpx"
										height="22rpx"
									></up-image>
								</view>
							</view>
						</view>
					</view>
				</template>
			</view>
		</view>
	</mModal>
</template>

<script setup lang="ts">
import { reactive, ref } from "vue";
import mModal from "/src/components/common/mModal.vue";
import dayjs from "dayjs";

const emits = defineEmits(["comfirm"]);

const oDay = dayjs();

const refModal = ref();

const data = reactive({
	curTab: "日期",
	curDate: "",
	curTimeRange: "",
	timeRange: [
		{
			val: "0900_0930",
			from: "09:00",
			to: "09:30",
		},
		{
			val: "0930_1000",
			from: "09:30",
			to: "10:00",
		},
		{
			val: "1030_1100",
			from: "10:30",
			to: "11:00",
		},
		{
			val: "1130_1200",
			from: "11:30",
			to: "12:00",
		},
		{
			val: "1400_1430",
			from: "14:00",
			to: "14:30",
		},
		{
			val: "1430_1500",
			from: "14:30",
			to: "15:00",
		},
		{
			val: "1433_1500",
			from: "14:30",
			to: "24:59",
		},
	],
	opt: {
		tabList: ["日期", "时间段"],
	},
});

// 验证过期
const verifyExpired = (item) => {
	const expireTime = item.to.split(":");
	const expiredH = new Date().getHours() > expireTime[0];
	const expiredMin = new Date().getMinutes() > expireTime[1];
	return expiredH || (expiredH && expiredMin);
};

const changeTab = (item: string) => {
	data.curTab = item;
};

// 选择日期
const changeDate = (res) => {
	data.curDate = res;
	data.curTab = "时间段";
};

// 选择时间段
const selectTimeRnage = (item) => {
	if (verifyExpired(item)) return;
	data.curTimeRange = item.val;
};

const handleComfirm = async () => {
	await emits("comfirm", data.curDate.fulldate);
};

const changeShow = (show?) => {
	refModal.value.changeShow(show);
};

const handleShowChange = (val) => {
	if (!val) return;

	data.curTab = "日期";
	// data.curTimeRange = data.timeRange[0].val;
};

defineExpose({
	changeShow,
	comfirm: handleComfirm,
});
</script>

<style lang="scss" scoped>
.modal-wrap {
	position: relative;

	.tab-wrap {
		position: sticky;
		top: 0;
		display: flex;
		justify-content: center;
		align-items: center;

		.tab-single {
			// padding: 10rpx 29rpx;
			padding: 6rpx 19rpx;
			border-radius: 26rpx;
			font-size: 32rpx;
			font-weight: 500;
			line-height: 32rpx;
		}
	}

	// 日历 强制适应宽度
	// todo 高度未同步缩放
	.calendar-wrap {
		transform: scale(0.85);
	}

	.time-range-wrap-outter {
		margin-top: 68rpx;

		.time-range-wrap {
			// display: flex;
			// justify-content: space-around;
			// align-items: center;
			display: grid;
			grid-template-columns: 1fr 1fr;
			padding: 23rpx;

			* {
				justify-self: center;
			}

			.expired-time {
				font-size: 28rpx;
				font-weight: normal;
				line-height: 28rpx;
				color: #999999;
			}

			.expired-time-normal {
				font-size: 28rpx;
				font-weight: normal;
				line-height: 28rpx;
				color: #333333;
			}

			.expired {
				font-size: 22rpx;
				font-weight: normal;
				line-height: 22rpx;
				color: #999999;
			}

			.checked {
				position: relative;
				width: 29rpx;
				height: 29rpx;
				background: #f7cf49;
				border-radius: 99rpx;

				display: flex;
				justify-content: center;
				align-content: center;
			}
		}
	}
}
</style>
