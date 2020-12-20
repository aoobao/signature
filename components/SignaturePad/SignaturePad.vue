<template>
	<u-popup safe-area-inset-bottom border-radius="25" closeable mode="bottom" v-model="show">
		<div class="container">
			<view class="title">签名</view>
			<view class="handCenter" :style="getStyle">
				<canvas
					v-if="show"
					class="hand-writing"
					disable-scroll
					@touchstart="uploadScaleStart"
					@touchmove="uploadScaleMove"
					@touchend="uploadScaleEnd"
					canvas-id="__signature__canvas"
				></canvas>
			</view>

			<view class="buttons">
				<span class="button button_rewrite" @click="rewrite">重签</span>
				<span class="button button_submit" @click="submit">提交</span>
			</view>
		</div>
		<u-toast ref="uToast" />
	</u-popup>
</template>

<script>
import Handwriting from './signature.js';
const CANVAS_ID = '__signature__canvas';
export default {
	data() {
		return {
			canvasId: CANVAS_ID,
			show: false,
			width: '100%',
			height: '500rpx'
		};
	},
	computed: {
		getStyle() {
			return `width:${this.width};height:${this.height};`;
		}
	},
	watch: {
		show(val) {
			if (!val) {
				// 关闭
				if (this.reject) {
					this.reject({ type: 'close' });
				}
			}
		}
	},
	methods: {
		sign({ width = '660rpx', height = '500rpx', color = '#000' } = {}) {
			return new Promise((resolve, reject) => {
				this.width = width;
				this.height = height;
				this.show = true;

				this.resolve = resolve;
				this.reject = reject;

				this.$nextTick(() => {
					let query = uni.createSelectorQuery().in(this);
					let ctx = uni.createCanvasContext(CANVAS_ID, this);

					this.handwriting = new Handwriting({
						lineColor: color,
						slideValue: 50, // 0, 25, 50, 75, 100
						canvasName: CANVAS_ID,
						ctx: ctx
					});

					query
						.select('.handCenter')
						.boundingClientRect(rect => {
							this.handwriting.setSize(rect);
						})
						.exec();
				});
			});
		},
		close() {
			this.show = false;
		},
		rewrite() {
			this.handwriting.clear();
		},
		submit() {
			let self = this;

			if (this.handwriting.isEmpty()) {
				// 未签字
				return this.$refs.uToast.show({
					title: '请在框内签字',
					type: 'error'
				});
			}

			uni.canvasToTempFilePath(
				{
					canvasId: CANVAS_ID,
					quality: 1.0,
					fileType: 'png',

					success(res) {
						// self.resolve(res);
						let path = res.tempFilePath;
						self.reject = null;
						self.resolve(path);
					},
					fail(err) {
						let reject = self.reject;
						self.reject = null;
						reject({ type: 'err', err: err });
					},
					complete() {
						// 失败关闭吧
						self.show = false;
					}
				},
				this
			);
		},
		uploadScaleStart(event) {
			this.handwriting.uploadScaleStart(event);
		},
		uploadScaleMove(event) {
			this.handwriting.uploadScaleMove(event);
		},
		uploadScaleEnd(event) {
			this.handwriting.uploadScaleEnd(event);
		},
		toJSON(e) {
			return e;
		}
	}
};
</script>

<style scoped>
.container {
	width: 100%;
	/* height: 822rpx; */
	position: relative;
	background-color: #fff;
}
.title {
	width: 100%;
	display: flex;
	justify-content: center;
	color: #3e3e3e;
	font-size: 38rpx;
	font-weight: bold;
	margin-top: 35rpx;
}
.handCenter {
	margin: 35rpx auto 0;
	border: 1px dashed #979797;
}
.hand-writing {
	width: 100%;
	height: 100%;
}

.buttons {
	width: 570rpx;
	margin: 40rpx auto;
	display: flex;
	justify-content: space-between;
}
/* .btn {
	width: 270rpx;
	height: 78rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	border-radius: 14rpx;
	font-size: 33rpx;
} */
.buttons .button {
	width: 200rpx;
	height: 78rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	border-radius: 14rpx;
	font-size: 33rpx;
}
.buttons .button.button_rewrite {
	border: 1px solid #ff3b3b;
	background-color: #ffffff;
	color: #ff4343;
}

.buttons .button.button_submit {
	background-color: #ff824e;
	color: #fff;
}
</style>
