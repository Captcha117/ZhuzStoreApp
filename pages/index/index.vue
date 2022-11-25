<template>
	<view class="content">
		<web-view ref="webview" :src="src" :webview-styles="webviewStyles" :update-title="false"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				src: "https://store.zhuz.work",
				webviewStyles: {
					progress: {
						color: "#5EC468",
					},
				},
				wv: null,
				canBack: false,
			}
		},
		onLoad() {

		},
		methods: {

		},
		onBackPress(options) {
			//let h = plus.webview.getWebviewById(wv.id)
			//let url = h.getURL()
			//let url = wv.getURL()
			if (this.canBack) {
				this.wv && this.wv.back()
				return true //不返回提示再按一次退出应用
			}
			return false
		},
		onReady() {
			// #ifdef APP-PLUS
			// 此对象相当于html5plus里的plus.webview.currentWebview()。在uni-app里vue页面直接使用plus.webview.currentWebview()无效
			let currentWebview = this.$scope.$getAppWebview()
			setTimeout(() => {
				this.wv = currentWebview.children()[0]
				this.wv.addEventListener('progressChanged', (e) => {
					this.wv.canBack((e) => {
						this.canBack = e.canBack
					})
				})
			}, 300); //如果是页面初始化调用时，需要延时一下
			// #endif
		}

	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
