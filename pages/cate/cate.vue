<template>
	<view>
		<view class="scroll-view-container">
			<!-- 左侧滑动区 -->
			<scroll-view class="scroll-left" scroll-y="true" :style="{height: wh + 'px'}">
				<block v-for="(item, i) in cateList" :key="i">
					<view :class="['left-scroll-item', i === active ? 'active' : '']" @click="activeChange(i)">{{item.cat_name}}</view>
				</block>
			</scroll-view>
			<!-- 右侧滑动区 -->
			<scroll-view class="scroll-right" scroll-y="true" :style="{height: wh + 'px'}" :scroll-top="scrollTop">
				<view class="right-scroll-item" v-for="(item, i) in cateLevel2" :key="i">
					<!-- 二级分类的标题 -->
					<view class="cate-lv2-title">{{item.cat_name}}</view>
					<!-- 二级分类下的三级分类列表 -->
					<view class="cate-lv3-list">
						<view class="cate-lv3-item" v-for="(item3, i3) in item.children" :key="i3" @click="gotoGoodList(item3)">
							<!-- 三级分类的图片 -->
							<image :src="item3.cat_icon"></image>
							<!-- 三级分类的文本 -->
							<text>{{item3.cat_name}}</text>
						</view>
					</view>
				</view>
			</scroll-view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wh: 0,
				cateList: [],
				active: 0,
				// 二级分类列表
				cateLevel2: [],
				scrollTop: 0
			};
		},
		onLoad() {
			const sysInfo = uni.getSystemInfoSync()
			this.wh = sysInfo.windowHeight - 50
			this.getCatelist()
		},
		methods: {
			async getCatelist() {
				const { data: res} = await uni.$http.get('/api/public/v1/categories')
				if(res.meta.status !== 200) return uni.$showMsg()
				this.cateList = res.message
				// 为二级分类赋值
				this.cateLevel2 = res.message[0].children
			},
			activeChange(i) {
				this.active = i
				// 重新为二级分类赋值
				this.cateLevel2 = this.cateList[i].children
				console.log(this.scrollTop)
				this.scrollTop = this.scrollTop === 0 ? 1 : 0
			},
			gotoGoodList(item) {
				uni.navigateTo({
					url: '/subpkg/goods_list/goods_list?cid=' + item.cat_id
				})
			}
		}
	}
</script>

<style lang="scss">
.scroll-view-container{
	display: flex;
	.scroll-left{
		width: 120px;
		.left-scroll-item{
			background-color: #f7f7f7;
			line-height: 60px;
			text-align: center;
			font-size: 12px;
		}
		.active {
		  background-color: #FFFFFF;
		  position: relative;

		  &::before {
			content: ' ';
			display: block;
			width: 3px;
			height: 30px;
			background-color: #C00000;
			position: absolute;
			top: 50%;
			left: 0;
			transform: translateY(-50%);
		  }
		}
	}
	.scroll-right{
		.right-scroll-item{
			.cate-lv2-title{
				font-size: 20px;
				font-weight: bold;
				text-align: center;
				margin-bottom: 10px;
			}
			.cate-lv3-list{
				display: flex;
				flex-wrap: wrap;
				.cate-lv3-item{
					width: 33.33%;
					display: flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					margin-bottom: 10px;
					image{
						width: 60px;
						height: 60px;
					}
					text{
						font-size: 12px;
					}
				}
			}
		}
	}
}
</style>
