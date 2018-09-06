<template>
    <div>
       <figure class="img-figure" :class="imgdata.isInverse? 'is-inverse' : '' " :style="figureStyle"  @click="centerme">
          <img :src="imgdata.imageURL" :alt="imgdata.filename" v-show="!imgdata.isInverse">
          <figcaption class="fig-caption">
              <h2 class="img-title"  v-show="!imgdata.isInverse">{{imgdata.fileName}}</h2>
           <div class="img-back" v-show="imgdata.isInverse" @click="handleclick">
                <p>
                  {{imgdata.desc}}
                </p>
            </div>
          </figcaption>

       </figure>
    </div>
</template>

<style lang="scss">
  .img-figure{
    position: absolute;
    width: 320px;
    height: 360px;
    margin: 0;
    padding: 40px;

    background-color: #fff;

    box-sizing: border-box;
    cursor: pointer;
    transform-origin: 0 50% 0;
    transform-style: preserve-3d;
    transition: transform .6s ease-in-out, left .6s ease-in-out, top .6s ease-in-out;

    &.is-inverse {
      transform: translate(320px) rotateY(180deg);
    }
    .fig-caption{
      text-align: center;  
      .img-title {
        margin: 20px 0 0 0;
        color: #a7a0a2;
        font-size: 16px;
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

        background-color: #fff;

        box-sizing: border-box;
        transform: rotateY(180deg) translateZ(1px);
        backface-visibility: hidden;

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
  props: ['imgdata'],
  computed: {
    // imgtop: this.imgdata.filename,
    // imgleft: this.imgdata.pos.left,
    figureStyle() {
    //   console.log(this.imgdata.pos.left)
      var styleObj = {
        top: this.imgdata.pos.top + 'px',
        left: this.imgdata.pos.left + 'px'
      }
      if (this.imgdata.rotate !== 0) {
        styleObj.transform = 'rotate(' + this.imgdata.rotate + 'deg)'
      }
      if (this.imgdata.isCenter) {
        styleObj.zIndex = 11
      }
      return styleObj
    }
  },
  mounted() {
    this.debug()
  },
  methods: {
    debug() {
    //   console.log(this.imgStyle)
    },
    centerme() {
      this.$emit('center', this.imgdata.index)
    },
    handleclick() {
    }
  }
}
</script>
