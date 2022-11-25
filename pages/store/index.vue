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

<style lang="scss">
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
	
	.uni-app--showtabbar uni-page-head[uni-page-head-type="default"]~uni-page-wrapper {
		height: calc(100% - 44px - env(safe-area-inset-top)) !important;
	}
	
	// .uni-app--showtabbar uni-page-wrapper:after {
	//     height: 80rpx;
	//     height: calc(80rpx + constant(safe-area-inset-bottom));
	//     height: calc(80rpx + env(safe-area-inset-bottom));
	// }
	
	uni-tabbar {
		.uni-tabbar {
			// tab 背景
			border-radius: 10px;
			margin: 20px;
			box-shadow: 0 0 10px #00000045;
			.uni-tabbar-border {
				// tabBar 上边框
				background-color: transparent !important; // tabBar 上边框的颜色
			}
		}

		.uni-tabbar~.uni-placeholder {
			height: 80rpx;
		}
	}
</style>
