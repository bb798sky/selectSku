<template>
	<view class="container">
		<view class="skuBox">
			<view class="color" v-for="(item,index) in skuData" :key="index">
				<view class="type_contnent_box">
					<view class="title">
						{{item.attr_name}}
					</view>
					<view class="color_item_container">
						<view class="color_item" 
						:class="{'active':isActive[index]==index+''+typeIndex}" 
						:data-id="index+''+typeIndex" 
						 v-for="(typeItem,typeIndex) in item.attrs" :key="typeIndex" 
						@tap="clickSku($event,typeIndex,typeItem,index)">
							{{typeItem.attr_name}}
						</view>
					</view>	
				</view>
			</view>
		</view>
		<navigator  url="../noStock/index"><button type="primary">没有库存的情况</button></navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {

				isActive: [],

				skuData: [],
				
				arr: []

			}
		},
		methods: {
			clickSku(e, typeIndex, typeItem, index) {
				console.log('typeitem', e, '规格列表index', index)
				/* 这里是两层数据 */
				// let arr = []
				for (let i = 0, len = this.skuData.length; i < len; i++) {
					/* 每条规格项里的规格子项 */
					for (let j = 0, len = this.skuData[i].attrs.length; j < len; j++) {
						if (e.target.dataset.id == i + '' + j) {
							this.arr[i] = i + '' + j
							this.isActive = this.arr.concat()
						}
					}
				}
				console.log('外层个位内层十位', this.isActive, this.arr)
			},
		},
		onLoad() {
			uni.request({
				url: 'https://www.easy-mock.com/mock/5cb08b01feb0256836781213/selectSkuData',
				data: {},
				header: {},
				success: (res) => {
					console.log(res.data);
					this.skuData = res.data
				},
				fail: (err) => {
					console.log(err)
				}
			});
		}

	}
</script>

<style lang="scss" scoped>
	.skuBox {
		height: 400upx;
		overflow-y: scroll;

		.color {
			height: 200upx;
			border-bottom: 1upx solid rgba($color: #000000, $alpha: .1);

			.title {
				font-size: .8rem;
			}

			.color_item_container {
				height: 100%;
				display: flex;
				flex-wrap: wrap;
				flex-direction: row;
				justify-content: start;
				margin-top: 40upx;

				.color_item {
					margin-right: 35upx;
					margin-bottom: 20upx;
					position: relative;
					width: 120upx;
					text-align: center;
					border: 1upx solid #444444;
					line-height: 1.1rem;
					font-size: .7rem;
					text-align: center;
					border-radius: 10upx;

				}

				.active {
					background: peachpuff;
					border: 1upx solid #E94D68;
				}
			}

		}
	}
</style>
