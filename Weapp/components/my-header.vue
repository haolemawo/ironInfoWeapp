<template>
	<view class="header" >
		<div class="status"></div>
		<view class="tab">
			<go-back v-if="goBack"></go-back>
			<image class="avatar" :src="avatar"></image>
			<view class="header-title">
				{{title}}
			</view>
			<scroll-view v-if="scroll" class="scroll-view_x" scroll-x="true"  scroll-left="120">
				<view class="scroll-view-item_x" v-for="(v,i) in listData" @click="checkoutIron(v)" :key="i">{{v.name}}</view>
			</scroll-view>
		</view>
		<slot></slot>
		<image v-if="showGif" src="https://zzes-1251916954.cos.ap-shanghai.myqcloud.com/wave.gif" class="wave-gif" mode=""></image>
	</view>
</template>

<script>
	import GoBack from './go-back.vue'
	import {mapState} from 'vuex'
	import {getHotIron} from '../api/api.js'
	export default {
		components: {
			GoBack
		},
		props: {
			title: {
				type: 'String'
			},
			showGif: {
				type: 'Boolean',
				default: false
			},
			goBack: {
				type: 'Boolean',
				default: false
			},
			scroll: {
				type: 'Boolean',
				default: false
			}
		},
		data() {
			return {
				listData: []
			}
		},
		computed:{
			...mapState({
				avatar: state => state.infos.avatarUrl
			})
		},
		mounted() {
			if(this.scroll) {
				this.getIronList()
			}
		},
		methods: {
			/**
			 * 获取热门钢材
			 */
			getIronList() {
				getHotIron().then(res => {
					this.listData = res.data
					this.listData.length = 6
				})
			},
			/**
			 * 选中钢材
			 */
			checkoutIron(v) {
				this.$emit('select', v)
			}
		}
	}
</script>

<style>
.status{  
	height: var(--status-bar-height);  
}
.header {
	height: auto;
	position: fixed;
	width: 100%;
	top: 0;
	z-index: 1024;
	box-sizing: border-box;
	text-align: center;
	color: #fff;
	background: #000131;
	font-size: 54rpx;
}
.header .header-title {
	display: inline-block;
	font-size: 28upx;
}
.header .tab {
	display: flex;
	height: 88upx;
	align-items: center;
	padding-left: 20upx;
	text-align: left;
	background: transparent;
}
.header .tab .avatar {
	margin-right: 20upx;
	width: 50upx;
	height: 50upx;
	border-radius: 50%;
}
.scroll-view_x {
	width:348upx;
	margin-left: 20upx;
	font-size: 24upx;
	white-space: nowrap;
}
.scroll-view-item_x {
	display: inline-block;
	padding: 0 20upx;
	margin-right: 20upx;
	text-align: center;
	border: 1upx solid #fff;
	border-radius: 2rem;
}
.tab-item {
	display: inline-block;
	font-size: 28upx;
}




</style>
