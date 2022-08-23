<template>
	<view class="my-search-container" :style="{'background-color':bgcolor}" @click="searchBoxHandler">
		<!-- 使用view组件模拟input输入框的样式 -->
		<view class="my-search-box" :style="{'border-radius':radius+'px'}">
			<uni-icons type="search"size="17"></uni-icons>
			<text class="placeholder">搜索</text>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			//背景颜色
			bgcolor:{
				type:String,
				default:'#C00000'
			},
			//圆角尺寸
			radius:{
				type:Number,
				default:18
			}
		},
		name:"my-search",
		data() {
			return {
				
			};
		},
		methods:{
			searchBoxHandler(){
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss">
.my-search-container{
	height: 50px;
	display: flex;
	align-items: center;
	padding: 0 10px;
	.my-search-box{
		height: 36px;
		background-color: #ffffff;
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		.placeholder{
			font-size: 15px;
			margin-left: 5px;
		}
	}
}
</style>