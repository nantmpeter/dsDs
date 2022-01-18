<template>
	<view>
		<uni-forms :modelValue="create">
			<uni-forms-item required label="下载链接" name="uri">
				<uni-easyinput type="text" v-model="create.uri" placeholder="请输入" />
			</uni-forms-item>
			<uni-forms-item required label="nas位置" name="destination">
				<uni-easyinput type="text" v-model="create.destination" placeholder="请输入" />
			</uni-forms-item>
		</uni-forms>
		<button @click="submitForm">创建</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				create: {
					uri: '',
					destination: 'data/Movies'
				}
			}
		},
		methods: {
			submitForm() {
				let sid = uni.getStorageSync('sid')
				let address = uni.getStorageSync('address')
				uni.request({
					url: address+'/webapi/DownloadStation/task.cgi',
					data: {
						_sid: sid,
						api: 'SYNO.DownloadStation.Task',
						version: '1',
						method: 'create',
						uri: this.create.uri,
						destination: this.create.destination
					},
					success: (res) => {
						console.log(res)
					}
				})
			}
		}
	}
</script>

<style>

</style>
