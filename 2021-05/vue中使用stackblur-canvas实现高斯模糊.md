### 说明

1. 通过[html2canvas](https://www.npmjs.com/package/html2canvas) 生成分享海报
1. 通过[stackblur-canvas](https://www.npmjs.com/package/stackblur-canvas) 实现高斯模糊背景

Q: 还有其他模糊方案么？
A: 基于团队以及项目实际情况考虑，曾采用[阿里云图片处理模糊效果](https://help.aliyun.com/document_detail/44701.html)

Q: 为什么最终没用，从性能角度看，直接返回高斯模糊后的图片不是更好？
A: 主要问题在于最终图片呈现的效果，oss在较高模糊度上图片显示不细腻，会有斑块感，例如这个[r_50,s_50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/blur,r_50,s_50)，再者考虑到海报生成的时候，实际客户容忍度会比较高，稍微慢一点问题不大

Q: 如何解决跨域问题？
A: 1.图片CDN配置允许跨域访问，设置 `img.crossOrigin='anonymous'`, 可参考下面 `convertURLToImage` 函数的实现


### 上代码

有些地方还可以优化，根据实际项目情况来调整

```vue
<template>
  <img :src="value" @load="$emit('load', $event)" />
</template>

<script>
export default {
  name: 'ImgBlur',
  props: {
    src: {
      type: String,
      default: '',
    },
    radius: {
      type: Number,
      default: 20,
    },
  },
  data() {
    return {
      value: '',
    }
  },
  async created() {
    this.value = await setBlur(this.src, this.radius)
  },
}

async function setBlur(url, radius = 0) {
  if (!url || !radius) return url
  const { canvasRGB } = await import('stackblur-canvas') // Nuxt 项目支持异步引入，避免撑大 vendor
  const img = await convertURLToImage(url)
  const canvas = convertImageToCanvas(img)
  canvasRGB(canvas, 0, 0, img.width, img.height, radius) // 最好提前判断是否包含 Alpha 通道 canvasRGBA
  const data = canvas.toDataURL('image/jpeg') // 这里 jpeg 转换效率 比 png 高不少，毕竟少了 aplha 通道的数据
  return data
}

function convertURLToImage(src) {
  return new Promise(resolve => {
    const img = new Image()
    img.onload = () => resolve(img)
    img.crossOrigin = 'anonymous' // 解决 Operation is insecure 跨域问题
    img.src = src // 实际测试 iOS 14.x 以下必须先设置 crossOrigin 再设置 src
  })
}

function convertImageToCanvas(img) {
  const elm = document.createElement('canvas')
  const ctx = elm.getContext('2d')
  elm.width = img.width
  elm.height = img.height
  ctx.drawImage(img, 0, 0)
  return elm
}
</script>
```