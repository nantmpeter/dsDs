<template>
	<view>
		<uni-forms :modelValue="login">
			<uni-forms-item required label="地址" name="address">
				<uni-easyinput type="text" v-model="login.address" placeholder="请输入地址" />
			</uni-forms-item>
			<uni-forms-item required label="用户名" name="username">
				<uni-easyinput type="text" v-model="login.username" placeholder="请输入账户" />
			</uni-forms-item>
			<uni-forms-item required label="密码" name="password">
				<uni-easyinput type="text" v-model="login.password" placeholder="请输入密码" />
			</uni-forms-item>
		</uni-forms>
		<button @click="submitForm">登录</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				login:{
					address: '192.168.31.22',
					username: 'wang',
					password: '54123789'
				}
			}
		},
		methods: {
			submitForm() {
				console.log(this.login)
				uni.setStorageSync('address','http://'+this.login.address+':5000')
				uni.request({
					url:'http://'+this.login.address+':5000/webapi/auth.cgi',
					data: {
						api: 'SYNO.API.Auth',
						version: '2',
						method: 'login',
						account: this.login.username,
						passwd: this.login.password,
						session: 'DownloadStation',
						format: 'sid'
					},
					success: (res) => {
						console.log(res.data.data.sid)
						uni.setStorageSync('sid', res.data.data.sid)
						uni.redirectTo({url: '/pages/taskList/taskList', fail(err){
							console.log(err)
						}})
						console.log(222)
					}
				})
			}
		}
	}
</script>

<style>

</style>
