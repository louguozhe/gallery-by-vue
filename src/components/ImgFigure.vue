<template>
  <figure class="img-figure" :style="figureStyle"  @click="clickImg">
    <img :src="imgdata.imageURL" :alt="imgdata.desc" >
      <figcaption class="fig-caption">
        <h2 class="img-title"  >{{imgdata.fileName}}</h2>
          <div class="img-back" :class="imgdata.isInverse? 'img-inverse': ''" :style="backStyle">
             <p>{{imgdata.desc}}</p>
          </div>
      </figcaption>
  </figure>
</template>

<style lang="scss">
.img-figure {
  position: absolute;
  margin: 0;
  padding: 14px;

  background: #F4F0EC;

  box-sizing: border-box;
  border: 6px double #483C32;
//   box-shadow: 0 0 0 50px rgba(125,135,18,0.3) inset; 

  cursor: pointer;
  transform-origin: 0 50% 0;
  transform-style: preserve-3d;
  transition: transform 0.6s ease-in-out, left 0.6s ease-in-out,
    top 0.6s ease-in-out;

  .fig-caption {
    text-align: center;
    .img-title {
      margin: 10px 0 0 0;
      line-height: 30px;
      color: #a7a0a2;
      font-size: 16px;
      background-color: antiquewhite;
    }
    .img-back {
      position: absolute;
      top: 0;
      left: 0;

      width: 100%;
      height: 100%;
      padding: 50px 40px;
      overflow: auto;

      color: #a7a0a2;
      font-size: 22px;
      line-height: 1.25;
      text-align: left;

      background-color: #F4F0EC;

      box-sizing: border-box;
      transform: rotateY(-180deg);
      backface-visibility: hidden;
      &.img-inverse {
        backface-visibility: visible;
      }

      p {
        margin: 0;
      }
    }
  }
}
</style>

<script>
export default {
  name: 'ImgFigure',
  props: ['imgdata', 'imgSize'],
  computed: {
    // imgtop: this.imgdata.filename,
    // imgleft: this.imgdata.pos.left,
    backStyle() {
      var styleObj = {}
      //   styleObj.backface.visibility = 'hidden'
      return styleObj
    },
    figureStyle() {
    //   console.log(this.imgdata.pos.left)
      var styleObj = {
        top: this.imgdata.pos.top + 'px',
        left: this.imgdata.pos.left + 'px',
        width: this.imgSize.width + 'px',
        height: this.imgSize.height + 'px'
      }
      if (this.imgdata.rotate !== 0) {
        styleObj.transform = 'rotate(' + this.imgdata.rotate + 'deg)'
      }
      if (this.imgdata.isInverse) {
        styleObj.transform = 'translate(' + styleObj.width + ') rotateY(180deg)'
      }
      if (this.imgdata.isCenter) {
        styleObj.zIndex = 11
      }
      return styleObj
    }
  },
  methods: {
    clickImg() {
      console.log('click-img')
      this.$emit('click-img', this.imgdata.index)
    }
  }
}
</script>
