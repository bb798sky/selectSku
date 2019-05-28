<template>
	<view class="container">
		<view class="skuBox" v-if="guigedata">
			<view class="color" v-for="(specs, n) in guigedata.apec_attr" :key="n">
				<view class="type_contnent_box">
					<view class="title">
						{{ specs.attr_name }}
					</view>
					<view class="color_item_container">
						<view class="color_item" :class="[
							oItem.isShow ? '' : 'disable',
							subIndex[n] == index ? 'active' : ''
						  ]"
						 :data-id="index + '' + index" 
						 :data-sku="oItem.attr_name" 
						 :data-skucode="oItem.id" 
						 v-for="(oItem, index) in specs.attrs"
						 :key="index" @click="specificationBtn(oItem.id, n, $event, index)">
							{{ oItem.attr_name }}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 01.规格选择
				selectArr: [], //存放被选中的值
				shopItemInfo: {}, //存放要和选中的值进行匹配的数据
				subIndex: [], //是否选中 因为不确定是多规格还是但规格，所以这里定义数组来判断
				// 02.规格显示
				skuShow: null,
				guigedata: {},
			};
		},

		onLoad() {
			uni.request({
				url: 'https://www.easy-mock.com/mock/5cdd6b00fd487e51be5eb0a0/',
				data: {},
				header: {},
				success: (res) => {
					this.guigedata = res.data.guige_data
					this.setData()
				},
				fail: (err) => {
					console.log(err)
				}
			});
		},

		methods: {
			setData() {
				let sku = this.guigedata.sku;
				for (let i = 0, len = sku.length; i < len; i++) {
					this.shopItemInfo[sku[i].attr_ids.replace(/\//g, ",")] = sku[i];
					 /* 
					 * 修改数据结构格式，用规格组成的id用逗号分割作属性名
					 *  */
				}
				this.checkItem();
			},

			// 01.规格选择
			specificationBtn(item, n, event, index) {
				/* 
				 * n是外层循环，index是内层循环
				 * item是数据项
				 * event事件对象
				 *  */
				if (this.selectArr[n] != item) {
					/* 
					* n是外层索引，也就是颜色、尺寸这样的规格类别
					* (this.selectArr[n] != item) 也就是这一类规格没有再点击一样的
					*  */
					this.selectArr[n] = item;
					this.subIndex[n] = index;
				} else {
					/* 
					* 如果是点击了这一类里刚点击了的规格，则去除选中态
					* 也就是点击了选中再点击去除选中
					* */
					this.selectArr[n] = "";
					this.subIndex[n] = -1; //去掉选中的颜色
				}
				this.checkItem();
			},
			checkItem() {
				let option = this.guigedata.apec_attr;
				let result = []; //定义数组存储已经被选中的值
				let len = option.length;
				for (let i = 0; i < len; i++) {
					result[i] = this.selectArr[i] ? this.selectArr[i] : "";
				}
				for (let i = 0; i < len; i++) {
					let last = result[i]; //把选中的值存放到字符串last去
					let leng = option[i].attrs.length;
					for (let k = 0; k < leng; k++) {
						result[i] = option[i].attrs[k].id; //赋值，存在直接覆盖，不存在往里面添加id值
						/* 
						* 循环判断每一个规格项
						* 在数据里面添加字段isShow来判断是否可以选择
						* 
						*  */
						option[i].attrs[k].isShow = this.isMay(result);
						/* 
						*  因为this.guigedata.apec_attr是一个对象，
						* option是指向这个对象的引用
						* 对option数据改动this.guigedata.apec_attr也会跟着改动
						* 用this.guigedata.apec_attr来渲染的oItem项数据也跟着变动
						* 
						*  */
						// console.log(this.guigedata.apec_attr[i].attrs[k].isShow,'===',option[i].attrs[k].isShow)
					}
					result[i] = last; //还原，目的是记录点下去那个值，避免下一次执行循环时避免被覆盖
				}
				this.$forceUpdate(); //重绘
			},
			isMay(result) {
				console.log(result)
				let len = result.length;
				for (let i = 0; i < len; i++) {
					if (result[i] == "") {
						return true; //如果数组里有为空的值，那直接返回true
					}
				}
				return this.shopItemInfo[result].stock == 0 ? false : true; //匹配选中的数据的库存，若不为空返回true反之返回false
			},
		},
	}
</script>

<style lang="scss" scoped>
	.skuBox {
		height: 800upx;
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

				.disable {
					pointer-events: none;
					background: lightgray;
					border: #444444;
				}
			}

		}
	}
</style>
