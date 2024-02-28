<template>
  <div
      :class="
      ['simon-square', {
          'simon-square_on': on,
          'simon-square_fail': fail,
          'simon-square_top-left': pos === 0,
          'simon-square_top-right': pos === 1,
          'simon-square_bottom-left': pos === 2,
          'simon-square_bottom-right': pos === 3,
        }
      ]"
      @click="$emit('click')">
  </div>
</template>

<script>
export default {
  name: "SimonSquare",
  props: {
    on: {
      type: Boolean,
      default: false
    },
    fail: {
      type: Boolean,
      default: false
    },
    color: {
      type: String,
      default: "red"
    },
    pos: {
      type: Number,
      default: 0
    }
  },
}
</script>


<style scoped>
.simon-square {
  width: 100px;
  height: 100px;
  border: 1px solid black;
  background-color: darkgrey;
  opacity: 0.5;
}

.simon-square_on {
  opacity: 1;
}

.simon-square_top-left {
  background-color: dodgerblue;
  border-top-left-radius: 100px;
}

.simon-square_top-right {
  background-color: #FF5643;
  border-top-right-radius: 100px;
}

.simon-square_bottom-left {
  background-color: #FEEF33;
  border-bottom-left-radius: 100px;
}

.simon-square_bottom-right {
  background-color: #BEDE15;
  border-bottom-right-radius: 100px;
}

</style>