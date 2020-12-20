<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<!-- <text class="title">{{title}}</text> -->
			<button @click="signature">签名</button>
		</view>
		<signature-pad ref="signature"></signature-pad>
		<u-image width="700rpx" height="500rpx" :src="path" v-if="path"></u-image>
	</view>
</template>

<script>
import SignaturePad from '@/components/SignaturePad/SignaturePad.vue';
export default {
	components: { SignaturePad },
	data() {
		return {
			path: null
		};
	},
	methods: {
		signature() {
			this.$refs.signature
				.sign({
					width: '700rpx',
					height: '500rpx',
					color: '#000'
				})
				.then(path => {
					// console.log(path);
					this.path = path
				})
				.catch(e => {
					console.log('取消签名', e);
				});
		}
	}
};
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
