<template>
	<view class="egg-twist">
		<!-- 抽奖机主体 -->
		<view class="egg-twist-container" :style="{scale:size}">
			<image src="../../static/niu-dan-ji.png" class="egg-twist-main-img" mode="aspectFit">
			</image>
			<image src="../../static/jin-yong-btn.png" class="egg-twist-btn" mode="aspectFit" v-if="disabled"
				@click="handleDisabled"></image>
			<template v-if="!start && !disabled && !getRes">
				<image src="../../static/not-start-btn.png" class="egg-twist-btn" mode="aspectFit" @click="handleStart"></image>
				<image src="../../static/xiao-shou.png" class="egg-twist-shou" mode="aspectFit" @click="handleStart"></image>
			</template>
			<image src="../../static/starting-btn.png" class="egg-twist-btn" mode="aspectFit" v-if="start || getRes"></image>
			<image src="../../static/chu-kou.png" class="egg-twist-chu-kou" mode="aspectFit">
			</image>
			<!-- 以下为扭蛋图 -->
			<image v-for="(giftImg,index) in defGiftImgList" :key="index" :src="giftImg"
				:class="['egg-twist-' + (index + 1),{gift:start}]" mode="aspectFit" />
			<!-- 抽取结果蛋 -->
			<image :src="resBallImg || `../../static/qiu${resLv}.png`" class="res-egg" mode="aspectFit" v-if="getRes">
			</image>
		</view>

		<!-- 抽奖结果 -->
		<view class="gift-res" v-if="showGiftRes">
			<view class="res-container">
				<image :src="notWinImg" mode="aspectFit" class="not-win-img" v-if="!giftRes.giftName">
				</image>
				<view v-else class="res-img-box">
					<image src="../../static/guang.png" mode="aspectFit" class="res-guang"></image>
					<image :src="giftRes.url" mode="aspectFit" class="res-img"></image>
					<view class="res-text">恭喜获得：{{giftRes.giftName}}</view>
				</view>
				<image src="../../static/que-ren.png" class="res-submit" mode="aspectFit" @click="handleOk">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			// 扭蛋机大小，最小0.1
			size: {
				type: Number,
				default: 1
			},
			// 是否禁用
			disabled: {
				type: Boolean,
				default: false
			},
			// 掉落几等奖，1-5
			resLv: {
				type: Number,
				default: 1
			},
			// 播放掉落动画时默认使用resLv掉落1-5号球，如果需要自定义掉落图片，传递图片url
			resBallImg: {
				type: String,
				default: ''
			},
			// 抽取结果
			giftRes: {
				type: Object,
				default: () => {
					return {
						giftName: null,
						url: ""
					}
				}
			},
			// 未中奖时显示的图片
			notWinImg: {
				type: String,
				default: '../../static/wei-zhong-jiang.png'
			},
			// 替换奖品列表图，将按顺序依次替换，最多可替换12张
			giftImgList: {
				type: Array,
				default: () => {
					return []
				}
			}
		},
		data() {
			return {
				start: false,
				getRes: false,
				showGiftRes: false,
				/**
				 * 默认扭蛋列表，顺序如下
				 * 0 - 2：礼物、手机、手表
				 * 3 - 7：数字球1 - 数字球5
				 * 8 - 9：紫球
				 * 10 - 11：蓝球
				 * 12：金蛋
				 */
				defGiftImgList: [
					'../../static/li-wu.png',
					'../../static/shou-ji.png',
					'../../static/shou-biao.png',
					'../../static/qiu1.png',
					'../../static/qiu2.png',
					'../../static/qiu3.png',
					'../../static/qiu4.png',
					'../../static/qiu5.png',
					'../../static/zi-qiu.png',
					'../../static/zi-qiu.png',
					'../../static/lan-qiu.png',
					'../../static/lan-qiu.png',
					'../../static/jin-dan.png'
				]
			}
		},
		created() {
			this.initGiftList()
		},
		methods: {
			initGiftList() {
				this.defGiftImgList.splice(0, this.giftImgList.length, ...this.giftImgList)
			},
			handleStart() {
				this.$emit('start', () => {
					console.log(this.giftRes);
					this.start = true
					setTimeout(() => {
						this.getRes = true
						this.start = false
						setTimeout(() => {
							this.getRes = false
							this.showGiftRes = true
						}, 3000)
					}, 2000)
				})
			},
			handleDisabled() {
				this.$emit('canNotStart')
			},
			handleOk() {
				this.showGiftRes = false
				this.$emit('ok')
			}
		}
	}
</script>

<style lang="scss" scoped>
	.egg-twist {
		.gift-res {
			position: fixed;
			width: 100vw;
			height: 100vh;
			top: 0;
			left: 0;
			z-index: 100;
			background: rgba(0, 0, 0, 0.6);

			.res-container {
				position: fixed;
				z-index: 20;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -60%);
			}

			.not-win-img {
				width: 100%;
				height: 600rpx;
			}

			.res-img-box {
				text-align: center;
				color: #fff;
				z-index: 30;
				height: 600rpx;
				width: 100vw;

				.res-guang,
				.res-img,
				.res-text {
					position: absolute;
					top: 50%;
					left: 50%;
					transform: translate(-50%, -50%);
				}

				.res-img {
					width: 300rpx;
					z-index: 50;
				}

				.res-text {
					width: 100%;
					font-size: 42rpx;
					top: 80%;
				}
			}

			.res-submit {
				width: 300rpx;
				height: 100rpx;
				display: block;
				margin: 150rpx auto 0;
			}
		}
	}

	.egg-twist-container {
		width: 100vw;
		position: relative;
		height: 800rpx;
		font-size: 16px;

		.egg-twist-main-img {
			width: 100%;
			height: 100%;
			position: relative;
			z-index: 5;
		}

		.egg-twist-btn {
			position: absolute;
			bottom: -4%;
			width: 285rpx;
			height: 23%;
			z-index: 10;
			right: 3.5em;
		}

		.egg-twist-chu-kou {
			position: absolute;
			width: 130rpx;
			height: 18%;
			z-index: 4;
			bottom: 2px;
			left: 24%;
		}

		.egg-twist-shou {
			position: absolute;
			z-index: 10;
			height: 15%;
			bottom: 0;
			right: 3em;
			width: 150rpx;
			transform-origin: right bottom;
			animation: shouAnima .8s linear 0s infinite alternate;
		}

		.egg-twist-1,
		.egg-twist-2,
		.egg-twist-3 {
			position: absolute;
			z-index: 2;
			width: 200rpx;
			height: 22%;

			&.gift {
				animation-duration: 0.5s !important;
			}
		}

		.egg-twist-1 {
			left: 10em;
			top: 15%;
			animation: 2s linear 0s infinite normal none running liwuAni;
		}

		.egg-twist-2 {
			left: 10em;
			top: 45%;
			animation: 2s linear 0.4s infinite normal none running shouji;
		}

		.egg-twist-3 {
			left: 5em;
			top: 33%;
			animation: 2s linear 0.4s infinite normal none running shouji;
		}

		.egg-twist-4,
		.egg-twist-5,
		.egg-twist-6,
		.egg-twist-7,
		.egg-twist-8,
		.egg-twist-9,
		.egg-twist-10,
		.egg-twist-11,
		.egg-twist-12,
		.egg-twist-13,
		.res-egg {
			position: absolute;
			z-index: 1;
			width: 100rpx;
			height: 10%;

			&.gift {
				animation-duration: 0.5s !important;
			}
		}

		.egg-twist-4 {
			left: 8em;
			top: 24%;
			animation: 2s linear 0.4s infinite normal none running qiu1;
		}

		.egg-twist-5 {
			right: 6em;
			top: 47%;
			animation: 2s linear 0.4s infinite normal none running qiu2;
		}

		.egg-twist-6 {
			top: 37%;
			left: 13em;
			animation: 2s linear 0.4s infinite normal none running qiu3;
		}

		.egg-twist-7 {
			left: 9em;
			top: 51%;
			animation: 2s linear 0.4s infinite normal none running qiu2;
		}

		.egg-twist-8 {
			right: 6em;
			top: 30%;
			animation: 2s linear 0.4s infinite normal none running qiu5;
		}

		.egg-twist-9 {
			left: 6em;
			top: 50%;
			z-index: 1;
			animation: 2s linear 0.4s infinite normal none running qiu3;
		}

		.egg-twist-10 {
			left: 9em;
			top: 45%;
			z-index: 1;
			animation: 2s linear 0.4s infinite normal none running qiu3;
		}

		.egg-twist-11 {
			right: 7em;
			top: 55%;
			z-index: 1;
			animation: 2s linear 0.4s infinite normal none running qiu2;
		}

		.egg-twist-12 {
			right: 6em;
			top: 40%;
			z-index: 1;
			animation: 2s linear 0.4s infinite normal none running qiu2;
		}

		.egg-twist-13 {
			left: 9em;
			top: 35%;
			z-index: 1;
			animation: 2s linear 0.4s infinite normal none running qiu1;
		}

		.res-egg {
			width: 70rpx;
			height: 7%;
			left: 10%;
			top: 35%;
			animation: 2s linear 1 normal forwards running resEgg;
		}
	}

	@keyframes shouAnima {

		0%,
		80%,
		100% {
			transform: rotate(0);
		}

		40% {
			transform: rotate(30deg);
		}
	}

	@keyframes liwuAni {

		0%,
		100% {
			transform: rotate(60deg) translateZ(0);
		}

		50% {
			transform: rotate(60deg) translate3d(10%, 0, 0);
		}
	}

	@keyframes shouji {

		0%,
		100% {
			transform: rotate(-30deg) translateZ(0);
		}

		50% {
			transform: rotate(-30deg) translate3d(10%, 0, 0);
		}
	}

	@keyframes qiu1 {

		0%,
		100% {
			transform: rotate(45deg) translateZ(0);
		}

		50% {
			transform: rotate(45deg) translate3d(40%, 0, 0);
		}
	}

	@keyframes qiu2 {

		0%,
		100% {
			transform: rotate(-120deg) translateZ(0);
		}

		50% {
			transform: rotate(-120deg) translate3d(40%, 0, 0);
		}
	}

	@keyframes qiu3 {

		0%,
		100% {
			transform: rotate(-40deg) translateZ(0);
		}

		50% {
			transform: rotate(-40deg) translate3d(30%, 0, 0);
		}
	}

	@keyframes qiu5 {

		0%,
		100% {
			transform: rotate(110deg) translateZ(0);
		}

		50% {
			transform: rotate(110deg) translate3d(30%, 0, 0);
		}
	}

	@keyframes resEgg {
		0% {
			transform: rotateZ(-180deg);
		}

		10% {
			left: 6.8%;
			transform: rotateZ(-90deg);
		}

		50% {
			left: 6.8%;
			top: 75%;
			transform: rotateZ(-90deg);
		}

		100% {
			width: 100rpx;
			height: 10%;
			z-index: 5;
			top: 87%;
			left: 30%;
			transform: rotateZ(40deg);
		}
	}
</style>