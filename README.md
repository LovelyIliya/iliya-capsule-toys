# iliya-capsule-toys
uniapp扭蛋机组件

## 示例图片

<img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\dai-chou-qu.png" alt="dai-chou-qu" style="zoom:50%;" /><img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\chou-qu-zhong.png" alt="chou-qu-zhong" style="zoom:50%;" /><img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\diao-luo.png" alt="diao-luo" style="zoom:50%;" /><img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\wei-zhong-jiang.png" alt="wei-zhong-jiang" style="zoom:50%;" /><img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\chou-qu-jie-guo.png" alt="chou-qu-jie-guo" style="zoom:50%;" /><img src="D:\devTool\web前端\个人项目\iliya-capsule-toys\static\exampleImg\bu-ke-chou-qu.png" alt="bu-ke-chou-qu" style="zoom:50%;" />

## 使用示例

```html
<template>
	<view class="content">
		<iliyaCapsuleToys :giftImgList="giftImgList" :disabled="disabled" :resLv="resLv" :giftRes="giftRes" :resBallImg="resBallImg" @start="start" @canNotStart="canNotStart" @ok="ok" />
	</view>
</template>

<script>
	import iliyaCapsuleToys from '../../components/iliya-capsule-toys/iliya-capsule-toys.vue';
	export default {
		components: {
			iliyaCapsuleToys
		},
		data() {
			return {
				giftRes: {
					giftName: null,
					url: ""
				},
				giftImgList: [],
				resBallImg: '',
				disabled: false,
				resLv: 1
			}
		},
		onLoad() {

		},
		methods: {
			start(fn) {
				this.giftRes.giftName = '预制菜'
				this.giftRes.url = '../../static/zi-qiu.png'
				fn()
			},
			canNotStart() {
				uni.showModal({
					title: '提示',
					content: '余额不足'
				})
			},
			ok() {
				console.log('确认中奖信息');
			}
		}
	}
</script>
```

## Props

| 参数名      | 类型    | 说明                                                         | 默认值                                                       | 可选值                                  |
| ----------- | ------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------- |
| size        | Number  | 扭蛋机大小                                                   | 1                                                            | 最小0.1，无上限                         |
| disabled    | Boolean | 是否禁用                                                     | false                                                        | true                                    |
| resLv       | Number  | 掉落几等奖                                                   | 1                                                            | 1/2/3/4/5                               |
| resBallImg  | String  | 默认是使用resLv显示1-5等奖的球，传递此参数可自定义掉落奖品图片 | /                                                            | 图片地址                                |
| giftRes     | Object  | 要显示的中奖结果图片和名称，中奖时赋值，不中奖不赋值         | {<br> giftName: null,<br>	url: "" <br>}                   | giftName：奖品名称<br>url：奖品图片地址 |
| notWinImg   | String  | 未中奖时显示的图片                                           | /static/wei-zhong-jiang.png                                  | 图片地址                                |
| giftImgList | Array   | 抽奖机里显示的扭蛋图片，一共13张图，如果需要替换扭蛋图片，按照顺序传递图片地址可依次替换：<br>* 0 - 2：礼物、手机、手表<br/>* 3 - 7：数字球1 - 数字球5<br/>* 8 - 9：紫球<br/>* 10 - 11：蓝球<br/>* 12：金蛋 | [<br/>'/static/li-wu.png',<br/>'/static/shou-ji.png',<br/>'/static/shou-biao.png',<br/>'/static/qiu1.png',<br/>'/static/qiu2.png',<br/>'/static/qiu3.png',<br/>'/static/qiu4.png',<br/>'/static/qiu5.png',<br/>'/static/zi-qiu.png',<br/>'/static/zi-qiu.png',<br/>'/static/lan-qiu.png',<br/>'/static/lan-qiu.png',<br/>'/static/jin-dan.png'<br/>				] | ['图片地址']，最多可传递13张            |

## Methods

`@start`开始抽取，点击抽取按钮时触发，回调播放动画函数，可以在此方法内调用抽奖接口，然后赋值抽奖结果（如果中奖的话），并调用回调函数触发动画

```html
<template>
	<iliyaCapsuleToys  :resLv="resLv" :giftRes="giftRes" @start="start"  />
</template>
<script>
	data() {
        return {
            giftRes: {
                giftName: null,
                url: ""
            },
            resLv: 1
        }
    },
    methods: {
        start(fn) {
        api().then(res=>{
           this.giftRes.giftName = res.data.name
            this.giftRes.url = res.data.url
            fn()
        })
        },
    }
</script>
```

`@canNotStart`点击禁言按钮时触发

```html
<template>
	<iliyaCapsuleToys  :resLv="resLv" :giftRes="giftRes" @canNotStart="canNotStart"  />
</template>
<script>
	data() {
        return {
            giftRes: {
                giftName: null,
                url: ""
            },
            resLv: 1
        }
    },
    methods: {
      	canNotStart() {
				uni.showModal({
					title: '提示',
					content: '余额不足'
				})
			},
    }
</script>
```

`@ok`点击确认抽取结果时触发

```html
<template>
	<iliyaCapsuleToys  :resLv="resLv" :giftRes="giftRes" @ok="ok"  />
</template>
<script>
	data() {
        return {
            giftRes: {
                giftName: null,
                url: ""
            },
            resLv: 1
        }
    },
    methods: {
      	ok() {
				console.log('确认中奖信息');
			}
    }
</script>
```
