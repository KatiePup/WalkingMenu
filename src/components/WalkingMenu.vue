<template>
  <div
    tabindex="-1"
    @keydown.left="moveLeft"
    @keyup.left="stopLeft"
    @keydown.right="moveRight"
    @keyup.right="stopRight"
    @keydown.enter="play"
  >
    <svg width="1800" height="800">
      <g class="track" :transform="housesTransform">
        <g
          v-for="(house, index) in houses.houseArr"
          :key="house.key"
          :transform="`translate(${defaultWidth},0)`"
          class="house"
        >
          <!-- :transform="`translate(${houses.houseArr[index - 1]?.width ?? 0},0)`" -->
          <component :is="house.component" />
        </g>
      </g>

      <g class="johno" :transform="johnoTransform" transform-origin="50 50">
        <Johno width="100" height="100" />
      </g>

      <g>
        <text v-show="enter" x="100" y="500">Press Enter</text>
        <text v-show="enter" x="100" y="530">to Play</text>
      </g>
    </svg>
  </div>
</template>

<script>
import House from '../assets/WordsearchHouse.svg?component'
import Johno from '../assets/Johno.svg?component'
import Road from '../assets/Road.svg?component'
import { shallowRef } from 'vue'

export default {
  components: {
    House,
    Johno,
  },

  data: function () {
    const defaultWidth = 600;

    // Setup some objects to go on the path...
    const roadObj = {
      type: 'path',
      width: defaultWidth,
      component: shallowRef(Road),
    }

    // Build our path...
    const inputArr = [
      {
        type: 'house',
        width: defaultWidth,
        component: shallowRef(House),
      },
    ]

    let houseArr = []

    switch (inputArr.length) {
      case 1:
        houseArr = [roadObj, ...inputArr, roadObj]
        break
      case 2:
        houseArr = [...inputArr, roadObj]
        break
      default:
        houseArr = inputArr
        break
    }

    return {
      defaultWidth,
      screen: {
        width: houseArr.length * defaultWidth,
        height: 800,
      },
      johno: {
        direction: 1,
        moving: false,
        x: 900,
        y: 610,
        speed: 5,
        // transform: 'translate(900,610) scale(1,1)',
      },
      houses: {
        originX: 600,
        houseArr: houseArr,
        // transform: 'translate(0,0)',
      },
      enter: false,
    }
  },

  computed: {
    johnoTransform() {
      return `translate(${
        this.johno?.x < 900
          ? this.johno?.x
          : this.screen.width - this.johno?.x < 900
          ? this.screen.width - this.johno?.x
          : 900
      },610) scale(${this.johno?.direction},1)`
    },
    housesTransform() {
      return `translate(${this.houses?.originX - this.johno?.x},0)`
    },
  },

  mounted() {
    this.move()
  },

  methods: {
    // calcTransform: function () {
    //   this.johno.transform = `translate(900,610) scale(${this.johno.direction},1)`
    //   this.houses.transform = `translate(${-this.johno.x},0)`
    // },
    checkEnterPrompt: function () {
      this.enter =
        100 < this.houses.originX - this.johno.x &&
        this.houses.originX - this.johno.x < 200

      // FIX alongside fixing game array
    },
    play: function () {
      if (this.enter) {
        this.johno.y = 400
      }
    },
    move: function () {
      if (this.moving) {
        this.johno.x += this.johno.direction * this.johno.speed
        
        // TODO: Consider writing (or finding) clamp function.
        this.johno.x =
          this.johno.x < 0
            ? 0
            : this.johno.x > this.screen.width
            ? this.screen.width
            : this.johno.x
      }
      this.checkEnterPrompt()
      // this.calcTransform()
      setTimeout(this.move, 20)
    },
    moveLeft: function () {
      this.johno.direction = -1
      this.moving = true
    },
    stopLeft: function () {
      if (this.johno.direction === -1) {
        this.moving = false
      }
    },
    moveRight: function () {
      this.johno.direction = 1
      this.moving = true
    },
    stopRight: function () {
      if (this.johno.direction === 1) {
        this.moving = false
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.johno {
  /* fill: palevioletred;*/
  transition: transform 100ms;
}
</style>
