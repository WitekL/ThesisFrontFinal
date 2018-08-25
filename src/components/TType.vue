<template>
  <div>
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
          <div class="ttype">
            <v-line :config="topLine"></v-line>
            <v-line :config="middleLine"></v-line>
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
    name: "TType",
    data() {
      return {
        middle: 0,
        left: 0,
        right: 0,
        windowWidth: window.innerWidth,
        windowHeight : window.innerHeight,

        rectLeft: {
          x: 300,
          y: 25,
          width: 70,
          height: 30,
          stroke: "black",
          fill: "white"
        },

        rectMid: {
          x: 385,
          y: 60,
          width: 30,
          height: 70,
          stroke: "black",
          fill: "white"
        },

        rectRight: {
          x: 430,
          y: 25,
          width: 70,
          height: 30,
          stroke: "black",
          fill: "white"
        },

        topLine: {
          points: [100, 40, 700, 40],
          stroke: "black",
          strokeWidth: "round",
          lineCup: "round",
          lineJoin: "round"
        },

        middleLine: {
          points: [400, 40, 400, 360],
          stroke: "black",
          strokeWidth: "round",
          lineCap: "round",
          lineJoin: "round"
        },

        bottomLine: {
          points: [100, 360, 700, 360],
        stroke: "black",
          strokeWidth: "round",
          lineCap: "round",
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
      },
    },

    computed: {
      leftNum: function() {
        return this.left;
      },

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

        setTimeout(function() {
          vm.$refs.rectLeft[1].getStage().setAttr('x', 200);
          vm.$refs.layer.getStage().draw();

          vm.$refs.rectLeft[2].getStage().setAttr('x', 100);
          vm.$refs.layer.getStage().draw();
        });
      },

      middle: function() {
        var vm = this;

        setTimeout(function() {
          vm.$refs.rectMid[1].getStage().setAttr('y', 160);
          vm.$refs.layer.getStage().draw();

          vm.$refs.rectMid[2].getStage().setAttr('y', 260);
          vm.$refs.layer.getStage().draw();
        });

      },

      right: function() {
        var vm = this;

        setTimeout(function() {
          vm.$refs.rectRight[1].getStage().setAttr('x', 530);
          vm.$refs.layer.getStage().draw();

          vm.$refs.rectRight[2].getStage().setAttr('x', 630);
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
  .leftWrap {
    float: left;
    margin: 20px;
  }

  .middleWrap {
    float: left;
    margin: 20px;
  }

  .rightWrap {
    float: left;
    margin: 20px;
  }
</style>
