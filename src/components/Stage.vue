<template>
    <section class="stage" ref="stage">
        <section class="img-sec">
            <img-figure v-for="(item,index) in imgsArrangeArr" :key="index" :imgdata="item" :imgSize="Constant.imgSize" @clickimg="rearrange"/>
        </section>
        <nav class="controller-nav">
            <control-unit v-for="(item,index) in imgsArrangeArr" :key="index" :imgdata="item" @clickimg="rearrange"></control-unit>
        </nav>
    </section>
</template>

<style lang="scss">
  .stage{
      width: 100%;
      height: 700px;
      position: relative;
  }
  .img-sec {
    position: relative;

    width: 100%;
    height: 100%;
    overflow: hidden;

    background-color: #ddd;
    perspective: 1800px;
  }
  .controller-nav {
    position: absolute;
    left: 0;
    bottom: 30px;
    z-index: 101;

    width: 100%;

    text-align: center;
  }
</style>

<script>
  import ImgFigure from '@/components/ImgFigure'
  import ControlUnit from '@/components/ControlUnit'
  // 获取图片相关的数据
  var imageDatas = require('../data/imageDatas.json')

  // 利用自执行函数， 将图片名信息转成图片URL路径信息
  imageDatas = (function genImageURL(imageDatasArr) {
    for (var i = 0, j = imageDatasArr.length; i < j; i++) {
      var singleImageData = imageDatasArr[i]
      singleImageData.index = i
      singleImageData.imageURL = '/static/images/' + singleImageData.fileName
      singleImageData.pos = {
        left: 0,
        top: 0
      }
      singleImageData.rotate = 0
      singleImageData.isInverse = false
      singleImageData.isCenter = false
    }
    return imageDatasArr
  })(imageDatas)

  // console.log(imageDatas)

  export default {
    name: 'Stage',
    components: {
      ImgFigure,
      ControlUnit
    },
    data() {
      return {
        Constant: {
          centerPos: {
            left: 0,
            right: 0
          },
          hPosRange: { // 水平方向的取值范围
            leftSecX: [0, 0],
            rightSecX: [0, 0],
            y: [0, 0]
          },
          vPosRange: { // 垂直方向的取值范围
            x: [0, 0],
            topY: [0, 0]
          },
          imgSize: {
            width: 280,
            height: 320
          }
        },
        imgsArrangeArr: imageDatas
      }
    },
    mounted() {
      this.calcConst()
      this.rearrange(-1)
    },
    methods: {
      calcConst() {
        var stageDOM = this.$refs.stage
        var stageW = stageDOM.scrollWidth
        var stageH = stageDOM.scrollHeight
        var halfStageW = Math.floor(stageW / 2)
        var halfStageH = Math.floor(stageH / 2)
        // var imgFigureDOM = this.$refs.imgFigure0[0]
        var halfImgW = Math.floor(this.Constant.imgSize.width / 2)
        var halfImgH = Math.floor(this.Constant.imgSize.height / 2)
        // 计算中心图片的位置点
        this.Constant.centerPos = {
          left: halfStageW - halfImgW,
          top: halfStageH - halfImgH
        }
        // console.log(this.Constant.centerPos.left)
        // 计算左侧，右侧区域图片排布位置的取值范围
        this.Constant.hPosRange.leftSecX[0] = -halfImgW
        this.Constant.hPosRange.leftSecX[1] = halfStageW - halfImgW * 3
        this.Constant.hPosRange.rightSecX[0] = halfStageW + halfImgW
        this.Constant.hPosRange.rightSecX[1] = stageW - halfImgW
        this.Constant.hPosRange.y[0] = -halfImgH
        this.Constant.hPosRange.y[1] = stageH - halfImgH

        // 计算上侧区域图片排布位置的取值范围
        this.Constant.vPosRange.topY[0] = -halfImgH
        this.Constant.vPosRange.topY[1] = halfStageH - halfImgH * 3
        this.Constant.vPosRange.x[0] = halfStageW - this.Constant.imgSize.width
        this.Constant.vPosRange.x[1] = halfStageW
        // console.log(this.Constant)
      },
      /*
       * 获取区间内的一个随机值
       */
      getRangeRandom(low, high) {
        return Math.floor(Math.random() * (high - low) + low)
      },

      /*
      * 获取 0~30° 之间的一个任意正负值
      */
      get30DegRandom() {
        return ((Math.random() > 0.5 ? '' : '-') + Math.floor(Math.random() * 30))
      },
      inverse(index) {
        // console.log('翻转:' + index + this.imgsArrangeArr[index].isInverse)
        this.imgsArrangeArr[index].isInverse = !this.imgsArrangeArr[index].isInverse
      },
      rearrange(centindex) {
        if (centindex < 0 || centindex >= this.imgsArrangeArr.length) {
          centindex = Math.floor(Math.random() * this.imgsArrangeArr.length)
        }
        // console.log(centindex)
        if (this.imgsArrangeArr[centindex].isCenter) {
          this.inverse(centindex)
          return
        }
        this.imgsArrangeArr.forEach(function name(value, index) {
          if (centindex === index) {
            value.pos.top = this.Constant.centerPos.top
            value.pos.left = this.Constant.centerPos.left
            value.isCenter = true
            value.rotate = 0
            // this.imgsArrangeArr[index] = {
            //   pos: this.Constant.centerPos,
            //   rotate: 0,
            //   isCenter: true
            // }
          } else {
            var hPosRangeLORX = null

            // 前半部分布局左边， 右半部分布局右边
            if (index < this.imgsArrangeArr.length / 2) {
              hPosRangeLORX = this.Constant.hPosRange.leftSecX
            } else {
              hPosRangeLORX = this.Constant.hPosRange.rightSecX
            }
            value.pos.top = this.getRangeRandom(this.Constant.hPosRange.y[0], this.Constant.hPosRange.y[1])
            value.pos.left = this.getRangeRandom(hPosRangeLORX[0], hPosRangeLORX[1])
            value.isCenter = false
            value.rotate = this.get30DegRandom()
          }
        }.bind(this))
        // console.log(this.imgsArrangeArr)
      }
    }
}
</script>
