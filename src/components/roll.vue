<!--
 * @file: 滚动
-->
<template>
  <div class="c_roll">
    <ul>
      <li v-for="(item,index) in dataList" :key="index" ref="rolling" class="c_roll_list-item" :class="{'c_roll_list-item-animate':isAnimate == true}">{{item}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name:'Roll',
  data(){
    return{
      dataList: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
      timer: null,
      isAnimate: true,
    }
  },
  mounted(){
    this.timer = setInterval(() => {
      this.scrollAnimate()
    }, 100);
  },
  destroyed(){
    clearInterval(this.timer)
  },
  methods: {
    scrollAnimate(){
      let rol = this.$refs.rolling
      rol[0].style.marginTop = '30px'
      this.isAnimate = !this.isAnimate
      setTimeout(()=>{
        this.dataList.push(this.dataList[0])
        this.dataList.shift()
        rol[0].style.marginTop = '0px'
        this.isAnimate = !this.isAnimate
      },0)
    },
  }
}
</script>

<style lang="less" scoped>
.c_roll{
  margin: 100px auto;
  overflow: hidden;
}
.c_roll ul{
  height: 40px;
  overflow: hidden;
}
.c_roll_list-item{
  width: 100%;
  list-style: none;
  height: 40px;
  line-height: 40px;
  text-align: center;
  font-size: 24px;
  font-weight: 400;
}
.c_roll_list-item-animate{
  transition: all 0.1s ease;
}

</style>