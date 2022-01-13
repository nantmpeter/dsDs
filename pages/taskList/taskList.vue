<template>
	<view>
		<uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" styleType="button" activeColor="#4cd964"></uni-segmented-control>
		<uni-list>
		    <uni-list-item v-for="(item, index) in this.list" :key="item.id"  :title="item.title" :show-badge="true" :badge-text="item.status" v-show="statusShowMap[item.status] == current?true:false" ></uni-list-item>
		</uni-list>
		 <!-- <uni-load-more @clickLoadMore="loadMore()" :status="more"></uni-load-more> -->
		 <uni-fab

			 @fabClick="create()"
		 ></uni-fab>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				count: 0,
				more: 'more',
				current: 0,
				items: [ '进行中','已完成', '暂停'],
				statusShowMap: {
					'downloading': 0,
					'finished': 1,
					'paused': 2
				}
			}
		},
		methods: {
			fetch(offset) {
				console.log(offset)
				let sid = uni.getStorageSync('sid')
				let address = uni.getStorageSync('address')
				uni.request({
					url: address+'/webapi/DownloadStation/task.cgi',
					data: {
						_sid: sid,
						api: 'SYNO.DownloadStation.Task',
						version: '1',
						method: 'list',
						additional: 'detail,file',
						// limit: 20,
						offset: offset
					},
					success: (res) => {
						console.log(res)
						this.list = [...this.list, ...res.data.data.tasks]
						this.count = this.count+20
					}
				})
			},
			loadMore() {
				this.fetch(this.count)
			},
			create() {
				uni.navigateTo({
					url:'../create/create'
				})
			},
			onClickItem(e) {
				console.log(e)
				this.current = e.currentIndex
			}
		},
		onLoad() {
			this.fetch(this.count)
		},
		// onReachBottom() {
		// 	this.fetch(this.count)
		// }
	}
</script>

<style>

</style>
