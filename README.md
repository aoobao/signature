# UNI-APP 签名画板封装

## 核心组件所在文件 `components/SignaturePad/SignaturePad.vue`

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
<video id="video" controls preload="none" >
      <source id="mp4" src="https://aoobao.github.io/signature/signature.mp4" type="video/mp4">
      <p>Your user agent does not support the HTML5 Video element.</p>
</video>
