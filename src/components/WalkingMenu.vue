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
      <g>
        <g class="house" :transform="housesTransform">
          <House />
        </g>
        <text v-show="enter" x="100" y="500">Press Enter</text>
        <text v-show="enter" x="100" y="530">to Play</text>

        <g class="johno" :transform="johnoTransform" transform-origin="50 50">
          <Johno width="100" height="100" />
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
import House from '../assets/WordsearchHouse.svg?component'
import Johno from '../assets/JohnoSVG.svg?component'


export default {
  components: {
    House,
    Johno,
  },

  data: function () {
    return {
      johno: {
        direction: 1,
        moving: true,
        x: 0,
        y: 610,
        speed: 5,
        // transform: 'translate(900,610) scale(1,1)',
      },
      houses: {
        // transform: 'translate(0,0)',
      },
      enter: false,
    }
  },

  computed: {
    johnoTransform() {
      return `translate(900,610) scale(${this.johno?.direction},1)`
    },
    housesTransform() {
      return `translate(${-this.johno?.x},0)`
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
      this.enter = 300 < this.johno.x && this.johno.x < 400
    },
    play: function () {
      if (this.enter) {
        this.johno.y = 400
      }
    },
    move: function () {
      if (this.moving) {
        this.johno.x += this.johno.direction * this.johno.speed
        this.johno.x =
          this.johno.x < 0 ? 0 : this.johno.x > 500 ? 500 : this.johno.x
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
