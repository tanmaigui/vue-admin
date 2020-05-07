<template>
  <svg :class="svgClass" aria-hidden="true">
    <use :xlink:href="iconName" />
  </svg>
</template>
<script>
export default {
  name: "SvgIcon",
  props: {
    iconClass:{
      type: String,
      default:''
    },
    className: {
      type: String,
      default:''
    }
  },
  data() {
    return{
      aa:"aaa"
    };
  },
  computed: {
    //计算属性，得出最终结果，并返回
    iconName() {
      return `#icon-${this.iconClass}`
    },
    svgClass(){
      if(this.className){
        return `svg-icon ${this.className}`
      }else{
        return `svg-icon`
      }
    }
  },
  methods:{

  },
  mounted(){
    
  }
};
</script>
<style lang="scss" scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  fill:currentColor;
}
</style>