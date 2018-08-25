<template>
  <div class="network-composer">
    <div id="select">
      <select class="left" v-model.number="left" v-on:change="emitQuantity">
        <option value=0>0</option>
        <option value=1>1</option>
        <option value=2>2</option>
        <option value=3>3</option>
      </select>

      <select class="middle" v-model.number="middle" v-on:change="emitQuantity">
        <option value=0>0</option>
        <option value=1>1</option>
        <option value=2>2</option>
        <option value=3>3</option>
      </select>

      <select class="right" v-model.number="right" v-on:change="emitQuantity">
        <option value=0>0</option>
        <option value=1>1</option>
        <option value=2>2</option>
        <option value=3>3</option>
      </select>
    </div>
    <div class="canvas">
      <v-stage :config="configKonva" ref="stage">
        <v-layer ref="layer">
          <div class="pitype">
            <v-line :config="topLine"></v-line>
            <v-line :config="leftLine"></v-line>
            <v-line :config="rightLine"></v-line>
            <v-line :config="bottomLine"></v-line>
            <v-rect :config="rectLeft" v-for="n in left" :key="n" ref="rectLeft"></v-rect>
            <v-rect :config="rectMid" v-for="n in middle" :key="10+n" ref="rectMid"></v-rect>
            <v-rect :config="rectRight" v-for="n in right" :key="20+n" ref="rectRight"></v-rect>
          </div>
        </v-layer>
      </v-stage>
    </div>
  </div>
</template>

<script>
  export default {
    name: "PiType",
    data() {
      return {
        middle: 0,
        left: 0,
        right: 0,
        windowWidth: window.innerWidth,
        windowHeight : window.innerHeight,

        rectLeft: {
          x: 215,
          y: 60,
          width: 30,
          height: 70,
          stroke: "black",
          fill: "white"
        },
        rectMid: {
          x: 250,
          y: 25,
          width: 70,
          height: 30,
          stroke: "black",
          fill: "white"
        },
        rectRight: {
          x: 525,
          y: 60,
          width: 30,
          height: 70,
          stroke: "black",
          fill: "white"
        },
        topLine: {
          points: [100, 40, 700, 40],
          stroke: "black",
          strokeWidth: "round",
          lineCap: "round",
          lineJoin: "round"
        },
        leftLine: {
          points: [230, 40, 230, 360],
          stroke: "black",
          strokeWidth: "round",
          lineCap: "round",
          lineJoin: "round"
        },
        rightLine: {
          points: [540, 40, 540, 360],
          stroke: "black",
          strokeWidth: "round",
          lineCap: "round",
          lineJoin: "round"
        },
        bottomLine: {
          points: [100, 360, 700, 360],
          stroke: "black",
          strokeWidth: "round", lineCap: "round",
          lineJoin: "round"
        }
      };
    },

    methods: {
      emitQuantity() {
        var quantities = {};
        quantities.left = this.left;
        quantities.middle = this.middle;
        quantities.right = this.right;

        this.$emit('element-quantities', quantities)
      }
    },

    computed: {
      configKonva: function() {
        let windowHeight = this.windowHeight;
        let windowWidth = this.windowWidth;

        let scale = windowWidth/520;

        let width = 250 * scale
        let height = 120 * scale

        return {
          width: width,
          height: height,
          scale: { x: scale/4, y: scale/4 }
        }
      }
    },

    watch: {
      left: function() {
        var vm = this;
        vm.$refs.layer.getStage().draw();

        setTimeout(function() {
          var index = vm.$refs.rectLeft.length-1
          var distance = index * 100;

          if(vm.$refs.rectLeft.length > 0) {
            vm.$refs.rectLeft[index].getStage().setAttr('y', 60+distance);
          }
          vm.$refs.layer.getStage().draw();
        });
      },

      middle: function() {
        var vm = this;
        vm.$refs.layer.getStage().draw();

        setTimeout(function() {
          var index = vm.$refs.rectMid.length-1
          var distance = index * 100;

          if(vm.$refs.rectMid.length > 0) {
            vm.$refs.rectMid[index].getStage().setAttr('x', 250+distance);
          }
          vm.$refs.layer.getStage().draw();
        });
      },

      right: function() {
        var vm = this;
        vm.$refs.layer.getStage().draw();

        setTimeout(function() {
          var index = vm.$refs.rectRight.length-1
          var distance = index * 100;

          if(vm.$refs.rectRight.length > 0) {
            vm.$refs.rectRight[index].getStage().setAttr('y', 60+distance);
          }
          vm.$refs.layer.getStage().draw();
        });
      }
    },

    mounted() {
      window.addEventListener('resize', () => {
        this.windowWidth = window.innerWidth;
        this.windowHeight = window.innerHeight;
      });
    }
  }
</script>

<style>
  .network-composer {
    text-align: center;
  }
  #select {
    width: 90%;
  }

  .canvas {
    padding-top: 30px;
    margin-left: 40px;
  }
</style>
