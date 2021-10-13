<template>
	<view>
		<view class="u-flex user-box u-p-30">
			<view class="u-m-r-10 avatar" @tap="previewImage">
				<u-avatar :src="vuex_user.avatar_url" size="140"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-18 u-p-b-20">{{userInfo.name}}</view>
				<view class="u-font-14 u-tips-color">邮箱: {{userInfo.email}}</view>
			</view>
		</view>
		
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="account" title="修改信息" @tap="jump('pages/center/baseInfo')"></u-cell-item>
			</u-cell-group>
		</view>
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="rmb-circle" title="所有订单"></u-cell-item>
				<u-cell-item icon="star" title="商品收藏"></u-cell-item>
				<u-cell-item icon="map" title="收货地址"></u-cell-item>
			</u-cell-group>
		</view>
		
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="reload" title="退出登录" @tap="showLoginout"></u-cell-item>
			</u-cell-group>
		</view>
		<!-- 退出登录模态框 -->
		<u-modal v-model="isShowLoginout" content="确定退出登录吗？" :show-cancel-button="true" @confirm="loginOut" @cancel="showLoginout"></u-modal>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isShowLoginout:false,
				userInfo:{}
			}
		},
		onShow() {
			// 判断是否处于登录状态、如果未登录跳转登录页
			this.$u.utils.isLogin()
			// 如果已经登录，拿取信息
			this.userInfo=this.vuex_user
		},
		methods: {
			// 图像预览
			previewImage(){
				uni.previewImage({
					urls:[this.userInfo.avatar_url]
				})
			},
			jump(path){
				this.$u.route(path)
			},
			// 出现模态框
			showLoginout(){
				this.isShowLoginout=!this.isShowLoginout
			},
			// 确定退出登录
			async loginOut(){
				// 请求api
				await this.$u.api.authLoginOut()
				
				uni.showToast({
					title:"退出成功！"
				})
				setTimeout(()=>{
					// 清除缓存token和用户信息
					this.$u.vuex("vuex_token",null)
					this.$u.vuex("vuex_user",{})
					
					// 跳转首页
					this.$u.route({type:"reLaunch",url:"pages/index/index"})
				},1500)
				
			}
			
		}
	}
</script>

<style scoped lang="scss">
page{
	background-color: #ededed;
}
.avatar{

}
.camera{
	width: 54px;
	height: 44px;
	
	&:active{
		background-color: #ededed;
	}
}
.user-box{
	background-color: #fff;
}
</style>
