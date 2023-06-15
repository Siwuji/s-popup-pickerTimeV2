<template>
	<view class="content">
		<view>当前日期时间格式为:{{dateOption.format}}</view>
		<input @focus="showPicker" placeholder="默认格式yyyy-MM-dd" :value="value" />
		<button @click="changeDateParms">年月日时分秒</button>
		<button @click="changeDateParms1">年月日时分?</button>
		<button @click="changeDateParms2">来个中文?</button>

		<view class="default">
			注释：<br />
			1.format默认为yyyy-MM-dd，可不传。<br />
			2.能通过uni编译的小程序应该都可以使用,因我只试了微信和钉钉，所以其它小程序需要自行测试。<br />
		</view>
		<sDatePicker ref="datePicker" @confirm="saveDate" :option="dateOption"></sDatePicker>
	</view>
</template>

<script>
	import sDatePicker from '@/components/s-popup-pickerTime/s-popup-pickerTime.vue';
	export default {
		data() {
			return {
				value: "2022-05-10",
				dateOption: {
					format: 'yyyy-MM-dd', //日期格式
					defaultValue: "2022-05-10", //非必填：默认时间
					minTime: '2022-01-11 05:12:01', //非必填:最小时间
					maxTime: '2023-06-02 12:23:30', //非必填:最大时间
					yearArea: [2020, 2023], //非必填:年份区间，如果minTime/maxTime有值，优先级高于yearArea
					showClearBtn: true //是否显示清除按钮
				}
			}
		},
		onLoad() {

		},
		components: {
			sDatePicker
		},
		methods: {
			showPicker() {
				this.$refs.datePicker.show();
			},
			saveDate(e) {
				//日期选择器【确认】事件
				// console.log(e)
				this.dateOption.defaultValue = e;
				this.value = e;
			},
			changeDateParms() {
				this.dateOption.format = "yyyy-MM-dd HH:mm:ss"
			},
			changeDateParms1() {
				this.dateOption.format = "yyyy-MM-dd HH:mm"
			},
			changeDateParms2() {
				this.dateOption.format = "yyyy年MM月dd日HH时mm分ss秒"
			}
		}
	}
</script>

<style>
	.content {
		padding: 24rpx;
	}

	.content>input {
		display: block;
		margin: 24rpx 0;
		padding: 12rpx 8rpx;
		border: 1px solid #EEEEEE;
		font-size: 26rpx;
	}

	.content>button {
		margin-bottom: 24rpx;
	}

	.default {
		color: #666666;
		font-size: 24rpx;
	}
</style>