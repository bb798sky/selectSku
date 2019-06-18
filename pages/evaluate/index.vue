<template>
  <view class="uni-rate">
 <EvaluateStar :value="1" @change="changeScope" />
  </view>
</template>

<script>
import EvaluateStar from "./components/evaluate-star/evaluate-star.vue";
export default {
  name: "uni-rate",
  components: {
    EvaluateStar
  },
	methods:{
		 changeScope(arg){
			 console.log(arg, '改变星星个数')
		 }
	}
  
};
</script>

<style>

</style>