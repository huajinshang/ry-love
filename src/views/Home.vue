<template>
  <div class="home">
    <div class="canvas-box">
      <canvas id="heartCanvas"></canvas>
    </div>
    <div id="bs"></div>

  </div>
</template>

<script>
  // @ is an alias to /src
  // import HelloWorld from '@/components/HelloWorld.vue'

  export default {
    name: 'home',
    data() {
      return {
        arr: [],
        r: 4,
        i: '',
        radian: '',           //弧度
        radianDecrement: '',  //弧度增量
        time: 10,             //每个点之间的时间间隔
        intervalId: '',
        num: 360,             //分割为 360 个点
        startRadian: Math.PI,
        ctx_brush: ""
      }
    },
    mounted: function () {
      let canvas = document.getElementById('heartCanvas')
      let ctx = canvas.getContext('2d');
      this.ctx_brush = ctx
      console.log(ctx)
      console.log(this.ctx_brush)
      this.startAnimation()
    },
    methods: {
      startAnimation() {
        //让画布撑满整个屏幕，-20是滚动条的位置，需留出。如滚动条出现则会挤压画布。

        let WINDOW_HEIGHT = document.documentElement.clientHeight - 20;

        let WINDOW_WIDTH = document.documentElement.clientWidth - 20;

        // this.ctx_brush.width = WINDOW_WIDTH;

        // this.ctx_brush.heigh = WINDOW_HEIGHT;

        this.drawHeart();
      },
      drawHeart() {

        this.ctx_brush.strokeStyle = "red";

        this.ctx_brush.lineWidth = 1;//设置线的宽度

        this.radian = this.startRadian;//弧度设为初始弧度

        console.log(this.radian)
        this.radianDecrement = Math.PI / this.num * 2;

        this.ctx_brush.moveTo(this.getX(this.radian), this.getY(this.radian));//移动到初始点

        this.i = 0;

        this.intervalId = setInterval(() => {
          this.printHeart()
        }, this.time);

      },
      printHeart() {

        this.radian += this.radianDecrement;

        this.ctx_brush.lineTo(this.getX(this.radian), this.getY(this.radian));//在旧点和新点之间连线

        // this.arr.push("X:" + this.getX(this.radian) + "<br/>Y:" + this.getY(this.radian) + "<br/>");

        this.i++;

        console.log(this.arr)
        this.ctx_brush.stroke();//画线
        console.log('i' + this.i + '----' + 'num' + this.num)
        if (this.i >= this.num) {

          clearInterval(this.intervalId);

          // document.getElementById("bs").innerHTML = this.arr.join("");//打印所有的XY坐标点。

        }

      },
      getX(t) {//由弧度得到 X 坐标

        return 100 + this.r * (16 * Math.pow(Math.sin(t), 3));

      },
      getY(t) {//由弧度得到 Y 坐标

        return 50 - this.r * (13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));

      }
    },
    components: {}
  }
</script>
<style scoped lang="less">
  .home, .canvas-box {
    width: 100%;
    height: 100%;
    text-align: center;
    /*background-color: black;*/
    #heartCanvas {
      height: 100%;
      display: block;
      /*background-color:aqua;*/
    }
  }
</style>
