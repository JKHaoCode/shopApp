<template>
	<view class="home">
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item) in swipers" :key="item.id" @click="navItem(item.url)">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<!-- dao hang -->
		<view class="nav">
			<view class="nav_item" v-for="(item, index) in navs" :key="index" @click="navItem(item.path)">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		
		<view class="hot_goods">
			<view class="tit">
				推荐商品
			</view>
			<goods-list @goodsItemClick="goGoodsDetail" :goods="goods"></goods-list>
		</view>
	</view>
</template>

<script>
	import goodsList from '@/components/uni-goods-nav/uni-goods-nav.vue';
	export default {
		data() {
			return {
				swipers: [],
				goods: [],
				navs: [
					{
						icon: 'iconfont icon-ziyuan',
						title: '黑马超市',
						path: '/pages/goods/goods'
					},
					{
						icon: 'iconfont icon-guanyuwomen',
						title: '联系我们',
						path: '/pages/contact/contact'
					},
					{
						icon: 'iconfont icon-tupian',
						title: '社区图片',
						path: '/pages/pics/pics'
					},
					{
						icon: 'iconfont icon-shipin',
						title: '学习视频',
						path: '/pages/videos/videos'
					}
				]
			}
		},
		onLoad() {
			this.getSwipers();
			this.getHotGoods();
		},
		methods: {
			async getSwipers() {
				const res = await this.$myRequest({
					url: '/api/getlunbo'
				}) 
				
				this.swipers = res.data.message;
			},
			async getHotGoods() {
				const res = await this.$myRequest({
					url: '/api/getgoods?pageindex=1',
				});
				// console.log(res)
				this.goods = res.data.message;
			},
			navItem(url) {
				// console.log(url)
				uni.navigateTo({
					url
				});
			},
			goGoodsDetail(id) {
				console.log(123123123)
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?id=' + id,
				})
			},
		}
	}
</script>

<style lang="scss">
	.home{
		swiper{
			width: 750rpx;
			height: 380rpx;
			image{
				height: 100%;
				width: 100%;
			}
		}
		.nav {
			display: flex;
			.nav_item{
				width: 25%;
				text-align: center;
				view{
					width: 120rpx;
					height: 120rpx;
					background-color: #DF5000;
					border-radius: 50%;
					margin: 10px auto;
					line-height: 120rpx;
					color: #fff;
					font-size: 50rpx;
				}
				.icon-tupian{
					font-size: 45rpx;
				}
				text{
					font-size: 30rpx;
				}
			}
		}
		.hot_goods {
			background-color: #eee;
			overflow: hidden;
			margin-top: 10px;
			.tit{
				height: 50px;
				line-height: 50px;
				color: $shop-color;
				text-align: center;
				letter-spacing: 20px;
				background-color: #fff;
				margin: 5px auto;
			}
			
		}
	}
</style>
