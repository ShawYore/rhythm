<template>
	<view class="container">
		<u-form ref="form" :model="formData">
			<u-form-item label="日期" prop="date" required>
				<u-input v-model="formData.date" placeholder="请选择日期" @click="showDatePicker = true" />
				<u-picker :show="showDatePicker" mode="time" @confirm="handleDateConfirm"
					@cancel="showDatePicker = false" />
			</u-form-item>

			<u-form-item label="收益金额" prop="amount" required>
				<u-input v-model="formData.amount" type="number" placeholder="请输入收益金额" />
			</u-form-item>

			<u-form-item label="备注" prop="remark">
				<u-input v-model="formData.remark" type="textarea" placeholder="请输入备注信息" />
			</u-form-item>

			<u-button type="primary" @click="submitForm">提交记录</u-button>
		</u-form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showDatePicker: false,
				formData: {
					date: '',
					amount: '',
					remark: ''
				},
				rules: {
					date: [{
						required: true,
						message: '请选择日期'
					}],
					amount: [{
							required: true,
							message: '请输入金额'
						},
						{
							pattern: /^-?\d+(\.\d+)?$/,
							message: '请输入有效数字'
						}
					]
				}
			};
		},
		methods: {
			handleDateConfirm(e) {
				this.formData.date = e.year + '-' + e.month + '-' + e.day;
				this.showDatePicker = false;
			},
			submitForm() {
				this.$refs.form.validate(valid => {
					if (valid) {
						uni.showToast({
							title: '提交成功'
						});
						setTimeout(() => {
							uni.navigateBack();
						}, 1500);
					}
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.container {
		padding: 30rpx;
		background: #f5f5f5;
		min-height: 100vh;

		.u-form-item {
			margin-bottom: 30rpx;
			background: #fff;
			padding: 20rpx;
			border-radius: 12rpx;
		}
	}
</style>