<template>
    <view class="remark-content">
      <view class="add-img-box">
        <view
          v-for="(item, index) in remarkImg"
          :key="index"
          class="img"
        >
          <image class="img-body" :src="item"></image>
        </view>
        <view class="add-img" @tap="addImg()">
          <view class="center-btn">&#xe602;</view>
          <view class="txt">添加图片</view>
        </view>
      </view>
  </view>
</template>
<script>
import Uploader from "./js/upload_img.js";
export default {
  data() {
    return {
		remarkImg:'',
		content:'',
      }
  },

  onLoad(option) {
  },

  methods: {
    addImg() {
      let upLoader = new Uploader();
      upLoader.choose_and_upload(4).then(value => {
		  console.log(value,'-----------')
			this.remarkImg = [...this.remarkImg,...value]
      });
    },
  }
};
</script>

<style lang="scss" scoped>
@font-face {
  font-family: "iconfont"; /* project id 1221660 */
  src: url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.eot");
  src: url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.eot?#iefix")
      format("embedded-opentype"),
    url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.woff2") format("woff2"),
    url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.woff") format("woff"),
    url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.ttf") format("truetype"),
    url("//at.alicdn.com/t/font_1221660_hwtn6sff5bo.svg#iconfont") format("svg");
}

%padding-left-right {
  padding-left: 30upx;
  padding-right: 30upx;
}
.remark-content {
  background: white;
  @extend %padding-left-right;
  padding-top: 20upx;
  padding-bottom: 20upx;
  font-size: 24upx;
  text-indent: 2upx;
  .remark-textarea {
    width: 100%;
    height: 156upx;
  }
  .add-img-box {
    width: 690upx;
    height: 160upx;
    display: flex;
    justify-content: flex-start;
    .img {
      min-height: 160upx;
      min-width: 160upx;
      box-shadow: 0upx 0upx 15upx 6upx rgba(27, 12, 3, 0.06);
      border-radius: 10upx;
      background: lightblue;
      margin-left: 8upx;
      margin-right: 8upx;
	  .img-body{
		  overflow: hidden;
		  height: 100%;
		  width: 100%;
	  }
    }
    .img:first-child {
      margin-left: 0upx;
    }
    .img:last-child {
      margin-right: 0upx;
    }
    .add-img {
      width: 100%;
      height: 160upx;
      box-shadow: 0upx 0upx 15upx 6upx rgba(27, 12, 3, 0.06);
      border-radius: 10upx;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #d1d1d1;

      .center-btn {
        width: 68upx;
        height: 68upx;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
        overflow: hidden;
        border: 2upx solid #d1d1d1;
        font-size: 40upx;
        color: #d1d1d1;
        text-align: center;
        margin-bottom: 10upx;
        font-family: iconfont;
      }
      .txt {
      }
    }
  }
}

</style>
