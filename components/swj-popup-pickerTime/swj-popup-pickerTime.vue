<template>
	<view class="pcaPicker" @touchmove.stop.prevent="preventTouchMove">
		<view :class="popFlag ? 'popMask' : ''" @click="hide"></view>
		<view class="popBottom" :class="popFlag ? 'popRound' : ''">
			<view class="head">
				<text class="t1" @click="hide">关闭</text>
				<text class="t3" @click="reset" v-if="option.showClearBtn">清空</text>
				<text class="t2" @click="confirm">确认</text>
			</view>
			<view ref="pickerDom">
				<picker-view v-if="visible" immediate-change=true :indicator-style="indicatorStyle" :value="value"
					@change="bindChange" class="picker-view">
					<picker-view-column v-for="(e,index) in types" :key="index">
						<view v-if="e == 'yyyy'" class="item" v-for="(yitem, yindex) in years" :key="yindex">
							{{ yitem }}年
						</view>
						<view v-if="e == 'MM'" class="item" v-for="(Mitem, Mindex) in months" :key="Mindex">{{ Mitem }}月
						</view>
						<view v-if="e == 'dd'" class="item" v-for="(ditem, dindex) in days" :key="dindex">{{ ditem }}日
						</view>
						<view v-if="e == 'HH'" class="item" v-for="(hitem, hindex) in hours" :key="hindex">{{ hitem }}时
						</view>
						<view v-if="e == 'mm'" class="item" v-for="(mitem, mindex) in mins" :key="mindex">{{ mitem }}分
						</view>
						<view v-if="e == 'ss'" class="item" v-for="(sitem, sindex) in secs" :key="sindex">{{ sitem }}秒
						</view>
					</picker-view-column>
				</picker-view>
			</view>
		</view>
	</view>
</template>

<script>
	import sPopupPickerTime from './swj-popup-pickerTime.js';
	export default {
		data() {
			return {
				popFlag: false,
				indicatorStyle: `height: 50px;`,
				pickertime: null,
				types: [],
				visible: false,
				value: [],
				years: [],
				months: [],
				days: [],
				hours: [],
				mins: [],
				secs: [],
				val: []
			};
		},
		props: {
			option: {
				type: Object,
				default: () => ({
					format: 'yyyy-MM-dd', //日期格式
					defaultValue: "2021-05-10",
					minTime: '1990-01-10',
					maxTime: '2023-06-01',
					showClearBtn: true
				})
			}
		},
		mounted() {},
		methods: {
			preventTouchMove() {
				// 防穿透
				return;
			},
			hide(e) {
				//隐藏
				this.popFlag = false;
				this.$emit('close', '');
			},
			show() {
				this.checkType();
				this.visible = true;
				this.popFlag = true;
			},
			reset() {
				this.$emit('confirm', '');
				this.popFlag = false;
			},
			confirm() {
				this.$emit('confirm', this.pickertime.getResult());
				this.popFlag = false;
			},
			bindChange(e) {
				const val = e.detail.value;
				this.val = val;
				this.pickertime.changeValue(val);
				this.renderData();
			},
			checkType() {
				this.pickertime = new sPopupPickerTime(this.option);
				this.value = this.pickertime.returnValue();
				this.types = this.pickertime.types;
				this.renderData();
			},
			renderData() {
				this.years = this.pickertime.years;
				this.months = this.pickertime.months;
				this.days = this.pickertime.days;
				this.hours = this.pickertime.hours;
				this.mins = this.pickertime.mins;
				this.secs = this.pickertime.secs;
			}
		}
	};
</script>

<style>
	.picker-view {
		width: 750rpx;
		height: 600rpx;
		margin-top: 20rpx;
	}

	.item {
		display: flex;
		height: 50px;
		align-items: center;
		justify-content: center;
		text-align: center;
	}

	.pcaPicker>.popMask {
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.6);
		z-index: 10000;
	}

	.pcaPicker>.popBottom {
		width: 100vw;
		background-color: #ffffff;
		font-size: 32rpx;
		position: fixed;
		bottom: 0;
		left: 0;
		transition: all 0.3s ease;
		transform: translateY(100%);
		opacity: 0;
		z-index: 20000;
	}

	.pcaPicker>.popRound {
		transition: all 0.3s ease;
		transform: translateX(0);
		opacity: 1;
	}

	.pcaPicker>.popBottom>.head {
		width: 100%;
		height: 80rpx;
		box-sizing: border-box;
		padding: 0 30rpx 0 30rpx;
		border-bottom: 1px solid #f0f0f0;
		overflow: hidden;
	}

	.pcaPicker>.popBottom>.head>text {
		display: block;
		width: 100rpx;
		height: 100%;
		text-align: center;
		line-height: 80rpx;
		font-weight: bold;
	}

	.pcaPicker>.popBottom>.head>.t1 {
		float: left;
		color: #888888;
	}

	.pcaPicker>.popBottom>.head>.t2 {
		float: right;
		color: #0070cc;
	}

	.pcaPicker>.popBottom>.head>.t3 {
		float: left;
		color: #ff0000;
	}

	.pcaPicker>.popBottom>picker-view {
		height: 500rpx;
	}

	.pcaPicker>.popBottom>picker-view view {
		text-align: center;
		line-height: 68rpx;
	}

	/* pad兼容 */

	@media (min-width: 520px) and (max-width: 900px) {
		.pcaPicker>.popBottom {
			width: 100vw;
			background-color: #ffffff;
			font-size: 16px;
			position: fixed;
			bottom: 0;
			left: 0;
			transition: all 0.3s ease;
			transform: translateY(100%);
			opacity: 0;
			z-index: 20000;
		}

		.pcaPicker>.popBottom>.head {
			width: 100%;
			height: 40px;
			box-sizing: border-box;
			padding: 0 15px 0 15px;
			border-bottom: 1px solid #f0f0f0;
			overflow: hidden;
		}

		.pcaPicker>.popBottom>.head>text {
			display: block;
			width: 100rpx;
			height: 100%;
			text-align: center;
			line-height: 40px;
			font-weight: bold;
		}

		.pcaPicker>.popBottom>.head>.t1 {
			float: left;
			color: #888888;
		}

		.pcaPicker>.popBottom>.head>.t2 {
			float: right;
			color: #0070cc;
		}

		.pcaPicker>.popBottom>.head>.t3 {
			float: left;
			color: #ff0000;
		}

		.pcaPicker>.popBottom>picker-view {
			height: 250px;
		}

		.pcaPicker>.popBottom>picker-view view {
			text-align: center;
			line-height: 34px;
		}

		.scheckbox-list {
			display: flex;
			align-items: center;
			padding: 12px;
		}

		.no-data {
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			padding: 50px 0;
			color: #666666;
			font-size: 15px;
		}

		.no-data>image {
			width: 100px;
			height: 100px;
			margin-bottom: 7px;
		}

		.no-data>button {
			margin: 12px 0 0 0;
			padding: 0 12px;
			font-size: 14px;
		}

		.no-data>button::after {
			display: none;
		}

		.pcaPicker-search {
			display: flex;
			align-items: center;
			width: 100%;
			height: 55px;
			padding: 11px 12px;
			background-color: #ffffff;
			border-bottom: 1px solid #eeeeee;
		}

		.pcaPicker-search>view {
			position: relative;
			flex: 1;
			height: 34px;
			padding: 0 12px 0 24px;
			color: #696969;
			font-size: 13px;
			background: #f2f2f2;
		}

		.pcaPicker-search>view::before {
			content: '';
			position: absolute;
			left: 6px;
			width: 15px;
			height: 32px;
			background: url('data:image/png;base64,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') no-repeat;
			background-size: 14px;
			background-position: center;
			z-index: 10;
		}

		.pcaPicker-search>view>.input-right-icon {
			position: absolute;
			width: 18px;
			height: 18px;
			top: 50%;
			margin-top: -9px;
			right: 12px;
		}

		.pcaPicker-search>button {
			padding: 0 12px;
			margin: 0;
			height: 34px;
			line-height: 34px;
			background-color: transparent;
			color: #000000;
			font-size: 13px;
			border: 0;
			outline: none;
		}

		.pcaPicker>.popRound {
			transition: all 0.3s ease;
			transform: translateX(0);
			opacity: 1;
		}
	}
</style>