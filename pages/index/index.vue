<template>
	<view class="content">
		<view>当前日期时间格式为:{{dateOption.format}}</view>
		<input @focus="showPicker" placeholder="默认格式yyyy-MM-dd" :value="dateOption.defaultValue" />
		<button @click="changeDateParms('yyyy-MM-dd HH:mm:ss')">年月日时分秒</button>
		<button @click="changeDateParms('yyyy-MM-dd HH:mm')">年月日时分?</button>
		<button @click="changeDateParms('MM-dd HH:mm')">月日时分</button>
		<button @click="changeDateParms('HH:mm:ss')">时分秒</button>
		<button @click="changeDateParms(('yyyy年MM月dd日 HH时mm分ss秒'))">来个中文?</button>

		<view class="default">
			注释：<br />
			1.format默认为yyyy-MM-dd，可不传。<br />
			2.能通过uni编译的小程序应该都可以使用,因我只试了微信和钉钉，所以其它小程序需要自行测试。<br />
			3.defaultValue/minTime/maxTime尽量和format格式相同<br />
			4.从时分秒转换为年月日时,记得将minTime/maxTime或者yearArea同步修改一下，否则可能切换后年份只有当年<br />
			5.如果minTime/maxTime有值，优先级高于yearArea<br />
		</view>
		<sDatePicker ref="datePicker" @confirm="saveDate" :option="dateOption"></sDatePicker>
	</view>
</template>

<script>
	import sDatePicker from '@/components/swj-popup-pickerTime/swj-popup-pickerTime.vue';
	export default {
		data() {
			return {
				dateOption: {
					format: 'yyyy-MM-dd', //日期格式
					defaultValue: "2022-05-06", //非必填：默认时间
					minTime: '', //非必填:最小时间
					maxTime: '', //非必填:最大时间
					yearArea: [2021,2025], //非必填:年份区间[2021,2025]，如果minTime/maxTime有值，优先级高于yearArea
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
			},
			changeDateParms(format) {
				this.dateOption.format = format
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