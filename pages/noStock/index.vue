<template>
	<view class="container">
		<view class="skuBox" v-if="guigedata">
			<view class="color" v-for="(item,index) in guigedata.apec_attr" :key="index">
				<view class="type_contnent_box">
					<view class="title">
						{{ item.attr_name }}
					</view>
					<view class="color_item_container">
						<view class="color_item" :class="{
                active: isActive[index] == index + '' + typeIndex,
                disable:
                  (typeItem.id == disableBtn[0] &&
                    isActive[index] != index + '' + typeIndex) ||
                  (typeItem.id == disableBtn[1] &&
                    isActive[index] != index + '' + typeIndex) ||
                  (typeItem.id == disableBtn[2] &&
                    isActive[index] != index + '' + typeIndex) ||
                  (typeItem.id == disableBtn[3] &&
                    isActive[index] != index + '' + typeIndex) ||
                  (typeItem.id == disableBtn[4] &&
                    isActive[index] != index + '' + typeIndex)
              }"
						 :data-id="index + '' + typeIndex" 
						 :data-sku="typeItem.attr_name" 
						  v-for="(typeItem, typeIndex) in item.attrs"
						 :key="typeIndex" 
						 @tap="clickGuige($event, typeItem)">
							{{ typeItem.attr_name }}
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
				guigedata: {},
				/* 没有库存的商品规格 */
				noStockSku: [],

				isActive: [],

				disable: [],

				disableBtn: [],
			};
		},

		onLoad() {
			uni.request({
				url: 'https://www.easy-mock.com/mock/5cdd6b00fd487e51be5eb0a0/',
				data: {},
				header: {},
				success: (res) => {
					this.guigedata = res.data.guige_data
					setData()
				},
				fail: (err) => {
					console.log(err)
				}
			});
			let setData = () => {
				const sku = this.guigedata.sku;
				for (let i = 0, len = sku.length; i < len; i++) {
					/* 库存为0的规格放入 */
					if (sku[i].stock == 0) {
						this.noStockSku.push({
							attr_ids: sku[i].attr_ids,
							attr_name: sku[i].attr_name
						});
					}
				}
			}
		},

		methods: {
			needDisableSku(typeItem) {
				let needDisableSkuCode = [];
				this.noStockSku.forEach(item => {
					let attr_name_arr = item.attr_name.split("/"),
						attr_ids_arr = item.attr_ids.split("/");
					if (attr_name_arr.includes(typeItem.attr_name)) {
						attr_ids_arr.forEach(item => {
							if (item != typeItem.id) {
								needDisableSkuCode.push(item);
							}
						});
					}
					function uniq(array) { /* 数组去重 */
						var temp = []; 
						for (var i = 0; i < array.length; i++) {
							if (temp.indexOf(array[i]) == -1) {
								temp.push(array[i]);
							}
						}
						return temp;
					}
					needDisableSkuCode = uniq(needDisableSkuCode);
					this.disableBtn = needDisableSkuCode;
				});
			},
			beSelectedSku(e) {
				for (let i = 0, len = this.guigedata.apec_attr.length; i < len; i++) {
					for (
						let j = 0, len = this.guigedata.apec_attr[i].attrs.length; j < len; j++
					) {
						if (e.target.dataset.id == i + "" + j) {
							this.isActive[i] = i + "" + j;
						}
					}
				}
			},
			clickGuige(e, typeItem) {
				if (this.disableBtn.includes(typeItem.id.toString())) return;
				this.needDisableSku(typeItem);
				this.beSelectedSku(e);
			},

		}
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
					background: lightgray;
					border: #444444;
				}
			}

		}
	}
</style>
