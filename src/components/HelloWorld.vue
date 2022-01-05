<!--
 * @Author: your name
 * @Date: 2022-01-05 21:20:33
 * @LastEditTime: 2022-01-05 23:40:52
 * @LastEditors: Please set LastEditors
 * @Description: 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
 * @FilePath: \timer-picker\src\components\HelloWorld.vue
-->
<template>
  <div class="hello">
    <div class="box">
      <div class="scale"></div>
      <ul
        class="scroll"
        @touchstart="touchstart"
        @touchmove="touchmove"
        @touchend="touchend"
        :style="getStyle"
      >
        <li class="normal" v-for="item in list" :key="item.key">{{ item.val }}</li>
      </ul>
    </div>

    <div class="companydiV">
            <img class="imgcompany" src="../assets/logo.png" alt="">
 </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      list: Array.from({ length: 380 }, (v, i) => ({key: i,val: i})),
      top: 0,
      marginTop: 0,
      touchStart: {
        top: 0,
        y: 0,
        start: 0
      },
      transition: 'none',
      isTouching: false
    };
  },
  methods: {
    touchstart(e) {
      const current = {
        x: e.targetTouches[0].pageX,
        y: e.targetTouches[0].pageY,
      };
      this.touchStart.top = this.top
      this.touchStart.y = current.y
      this.touchStart.start = Date.now()
      this.isTouching = true
      // console.log("touchstart", current);
    },
    touchmove(e) {
      const current = {
        x: e.changedTouches[0].pageX,
        y: e.changedTouches[0].pageY,
      };
      this.top = this.touchStart.top + current.y - this.touchStart.y
      // console.log("touchmove", current);
    },
    touchend(e) {
      const current = {
        x: e.changedTouches[0].pageX,
        y: e.changedTouches[0].pageY,
      };
      const miles = Date.now() - this.touchStart.start
      const distance = current.y - this.touchStart.y
      // console.log("touchend", current);
      const lv = Math.abs(distance) / miles

      let multi = 0
      let duration = 0.75
      this.isTouching = false
      // 
      if(lv > 0 && lv <= 0.2) {
        multi = 0.15
        duration *= 3
        // const duration = miles * 6 / 1000 > 0.75 ? miles * 6 / 1000 : 0.75
        this.transition = `transform ${duration}s cubic-bezier(.29,.59,.49,.97)`
      }else if(lv > 0.2 && lv <= 0.4) {
        multi = 0.35
        duration *= 2
        // const duration = miles * 6 / 1000 > 0.75 ? miles * 6 / 1000 : 0.75
        this.transition = `transform ${duration}s ease-out`
      } else if(lv > 0.4 && lv <= 0.8) {
        multi = 1
        this.transition = `transform ${duration}s ease-out`
      } else if(lv > 0.8 && lv <= 1.2) {
        multi = 1.5
        this.transition = `transform ${duration}s ease-out`
      } else {
        multi = 2
        this.transition = `transform ${duration}s ease-out`
      } 

      this.top = this.top + distance * (multi + 1)
      console.log('distance:',distance,';miles:',miles,';ratio:' ,lv ,',multi:',multi,'', ',duration:',duration)
    },
  },
  computed: {
    getStyle() {
      return  {
        transform: `translate3d(0,${this.top}px,0)`,
        transition: this.isTouching ? '' :this.transition
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.first {
  height: 50px;
}
.second {
  height: 45px;
}
.normal {
      list-style-type: none;
  height: 40px;
}
.box {
  position: relative;
  height: 220px;
  overflow: hidden;
  background: #c1c1c1;
}
.scroll {
  /* height: 220px; */
  /* overflow: scroll; */
  position: relative;
}
.scale {
  height: 50px;
  top: 90px;
  width: 100%;
  position: absolute;
  filter: blur(23px);
    z-index: 99999;
}
.companydiV{
    position: absolute;
    top: 130px;
    /* left: 80px; */
    transform: skewY(0) rotateZ(88deg) rotateX(8deg) rotateY(-79deg);
  }
  @keyframes rollImg {
    0% {
      transform :rotateZ(5deg);
    }
    100% {
      transform :rotateZ(1000deg);
    }
  }
  .imgcompany{
    /* transform: rotate(25deg); */
    animation:2s rollImg linear infinite normal;
    transform: skewX(-45deg);
  }
</style>
