<template>
	<view>
		<view class="search-container" :style="{'background-color': bgcolor}" @click="searchHandler">
			<view class="search-box" :style="{ 'border-radius': radius + 'px' }">
				<uni-icons type="search" size="17"></uni-icons>
				<text class="placeholder">搜索</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-search",
		data() {
			return {
				
			};
		},
		props: {
			// 背景颜色
			bgcolor: {
				type: String,
				default: "#c00000"
			},
			// 圆角尺寸
			radius: {
				type: Number,
				default: 18
			}
		},
		methods: {
			searchHandler() {
				this.$emit('gotoSearch')
			}
		}
	}
</script>

<style lang="scss">
.search-container{
	height: 50px;
	background-color: #c00000;
	display: flex;
	align-items: center;
	padding: 0 10px;
	.search-box{
		height: 36px;
		background-color: #ffffff;
		border-radius: 18px;
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		.placeholder {
			font-size: 15px;
			margin-left: 5px;
		  }
	}
}

</style>