# UNI-APP 签名画板封装

## 核心组件所在文件 `components/SignaturePad/SignaturePad.vue`
### 组件中有使用到uview-ui的popup组件,使用前需要引入uview组件库
[https://www.uviewui.com/](https://www.uviewui.com/)

## 基本使用方法
可参考页面 `pages/index/index`

### 将组件引入到template中

````
<template>
  <view>
    ....
		<signature-pad ref="signature"></signature-pad>
  </view> 
</template>

````

### js代码调用
````
this.$refs.signature
  .sign({
    width: '700rpx',  // 画板宽度
    height: '500rpx', // 画板高度
    color: '#000'     // 画笔宽高
  })
  .then(path => {
    console.log(path); // 图片输出路径(h5页面为base64)
  })
  .catch(e => {
    console.log('取消签名', e);
  });

````

## 效果展示
[https://aoobao.github.io/signature/signature.mp4](https://aoobao.github.io/signature/signature.mp4)
