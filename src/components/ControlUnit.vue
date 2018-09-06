<template>
  <span class="controller-unit" :class="classname"  @click="handleClick"></span>
</template>

<style lang="scss">
  @font-face {
    font-family: "icons-turn-arrow";
    src: url("../fonts/icons/turn-arrow.eot") format("embedded-opentype"), url("../fonts/icons/turn-arrow.woff") format("woff"), url("../fonts/icons/turn-arrow.ttf") format("truetype"), url("../fonts/icons/turn-arrow.svg") format("svg");
  }
  .controller-unit{
      display: inline-block;
            margin: 0 5px;
            width: 30px;
            height: 30px;

            text-align: center;
            vertical-align: middle;

            cursor: pointer;
            background-color: #aaa;
            border-radius: 50%;

            transform: scale(.5);
            transition: transform .6s ease-in-out, background-color .3s;
            
            &.is-center {
                background-color: #888;
                transform: scale(1);
                &::after {
                    color: #fff;
                    font-family: "icons-turn-arrow";
                    font-size: 80%;
                    line-height: 30px;
                    content: "\e600";
                    -webkit-font-smoothing: antialiased;
                    -moz-osx-font-smoothing: grayscale;
                }
                &.is-inverse {
                    background-color: #555;
                    transform: rotateY(180deg);
                }
            }
  }
</style>

<script>
export default {
  name: 'ControlUnit',
  props: ['imgdata'],
  computed: {
    classname() {
      var name = ''
      if (this.imgdata.isCenter) {
        name += 'is-center'
      }
      if (this.imgdata.isInverse) {
        name += ' is-inverse'
      }
      return name
    }
  },
  methods: {
    handleClick() {
      this.$emit('center', this.imgdata.index)
    }
  }
}
</script>
