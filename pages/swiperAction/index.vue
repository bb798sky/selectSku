<template>
	<view>
		<uni-swipe-action :options="options">
			<view class='cont'>SwipeAction 基础使用场景</view>
		</uni-swipe-action>
	</view>
</template>


<script>
	import uniSwipeAction from "./components/uni-swipe-action.vue";
	export default {
		components: {
			uniSwipeAction
		},
		data() {
			return {
				options: [{
					text: '取消',
					style: {
						backgroundColor: '#007aff'
					}
				}, {
					text: '确认',
					style: {
						backgroundColor: '#dd524d'
					}
				}]
			}
		}
	}
</script>

<style>
	
</style>
