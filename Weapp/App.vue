<script>
	import Vue from 'vue'
	import { countUser, getHotIron } from 'api/api.js'
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			// #ifdef MP-WEIXIN
			// 获取是否授权状态, 没授权跳登录页
			try {
				const token = uni.getStorageSync('token');
				if (token === '' || token === undefined || token === null) {
					uni.getSetting({
						success: (res) => {
							let status = res.authSetting['scope.userInfo']
							this.$store.commit('SET_INFO', status)
							// console.log(status)
							if (!status) {
								uni.redirectTo({
									url: '/pages/auth/auth'
								})
							} else {
								this.getInfo()
							}
						}
					})
				} else {
					this.getInfo()
				}
			} catch (err) {}
			// #endif
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		},
		methods: {
			getInfo() {
				uni.getUserInfo({
					success: (res) => {
						// 更新访问记录
						let openId = uni.getStorageSync('openId')
						let history = uni.getStorageSync('hot') ? uni.getStorageSync('hot') : '{}'
						countUser(openId, JSON.parse(history)).then(res => {
							console.log(res.data)
						}).catch(e => {})
						// 存储头像信息
						this.$store.commit('SAVE_INFO', res.userInfo)
					}
				})
			}
		}
	}
</script>

<style>
	@import 'common/uni.css';

	@font-face {
		font-family: iconfont;
		src: url('~@/static/iconfont.ttf');
	}

	/*每个页面公共css */
	page {
		min-height: 100%;
		display: flex;
		flex-direction: column;
		/* justify-content: center; */
	}

	.iconfont {
		font-family: iconfont;
	}


	.wave-gif {
		position: absolute;
		width: 100%;
		bottom: 0;
		left: 0;
		z-index: 99;
		mix-blend-mode: screen;
		height: 100upx;
	}
</style>