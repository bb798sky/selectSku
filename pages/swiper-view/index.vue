<template>
	<view class="container">
		<scroll-view 
		class="scroll-container" 
		scroll-x 
		scroll-with-animation 
		v-if="brands.length > 1" 
		:scroll-into-view="'s' + currentIndex">
			<view :class="['scroll-item', { active: index == currentId }]" 
			:id="'s' + index" 
			v-for="(item, index) in brands"
			:key="index" 
			@tap="handleScroll(index)">
			{{ item }}
				<view class="bottom-border" v-if="index == currentId">
					<!-- 底部红色短border -->
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentId:'',/* 控制被选中 */
				currentIndex:'',/* 控制位置 */
				brands:[
					'牛仔裤',
					'羽绒服',
					'皮鞋',
					'运动鞋',
					'跑步鞋',
					'篮球鞋',
					'围巾',
					'西装',
					'西裤'
				]
			};
		},

		onLoad() {

		},

		methods: {
			handleScroll(index) {
				this.currentId = index;
				this.currentIndex = Math.max(0, index - 1);
			},
		},
	}
</script>

<style lang="scss" scoped>
.scroll-container {
    box-sizing: border-box;
    padding-left: 20upx;
    white-space: nowrap;
    height: 80upx;
    background: #fff;
    border-bottom: 1upx solid #ccc;
    .scroll-item {
      text-align: center;
      display: inline-block;
      padding: 0 30upx;
      line-height: 70upx;
      font-size: 36upx;
      font-weight: 400;
      color: #1e1e1e;

      &.active {
        display: inline-block;
        font-weight: bold;
        color: #1e1e1e;
        font-size: 42upx;
        // border-bottom: 2px solid #FF4F52;
      }
    }

    .bottom-border {
      margin: 0 auto;
      width: 50upx;
      height: 10upx;
      background: rgba(255, 79, 82, 1);
      border-radius: 5upx;
    }
  }
</style>
