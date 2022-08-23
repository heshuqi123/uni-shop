<template>
	<view>
		<view class="goods-list">
			<block v-for="(goods,i) in goodsList" :key="i">
				<view @click="gotoDetail(goods)">
					<my-goods :goods="goods"></my-goods>
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				queryObj:{
					query:'',
					cid:'',
					pagenum:1,
					pagesize:10
				},
				
					
				//商品列表的数据
				goodsList:[],
				total:0,
				//节流阀
				isLoading:false
			}
		},
		onLoad(options) {
			this.queryObj.query = options.query || ''
			this.queryObj.cid = options.cid || ''
			
			this.getGoodsList()
		},
		methods: {
			async getGoodsList(cb){
				//打开节流阀
				this.isLoading=true
				cb&&cb()
				const{data:res} = await uni.$http.get('/api/public/v1/goods/search',this.queryObj)
				
				//关闭节流阀	
				this.isLoading=false
				if(res.meta.status !== 200) return uni.$showMsg()
				
				this.goodsList = [...this.goodsList, ... res.message.goods]
				this.total = res.message.total
			},
			gotoDetail(goods){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id='+goods.goods_id
				})
			}
		},
		onReachBottom() {
			if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg("数据加载中...")
			
			if(this.isLoading) return
			
			//让页码自增+1
			this.queryObj.pagenum++
			this.getGoodsList()
		},
		onPullDownRefresh() {
			//重置关键数据
			this.queryObj.pagenum = 1
			this.total = 0
			this.isLoading = false
			this.goodsList = []
			
			//重新发起数据请求
			this.getGoodsList(() => uni.stopPullDownRefresh())
		}
	}
</script>

<style lang="scss">

</style>
