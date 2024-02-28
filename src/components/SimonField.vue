<template>
<div class="simon-field">
  <simon-square
      v-for="(cell, idx) of lights"
      :on="cell.on"
      :fail="cell.fail"
      :pos="idx"
      @click="onClickHandler(idx)"
  />
</div>
</template>

<script>
import SimonSquare from "@/components/SimonSquare.vue";

export default {
  name: "SimonField",
  components: {SimonSquare},
  props: {
    steps: {
      type: Array,
      default: () => []
    },
    lightTime: {
      type: Number,
      default: 1000,
    }
  },
  data() {
    return {
      userMove: false,
      nextStep: 0,
      prevStep: null,
      intervals: {
        prevStepLight: null,
        success: null,
        nextStepOn: null,
        nextStepOff: null
      },
      lights: [
        {on: false, fail: false},
        {on: false, fail: false},
        {on: false, fail: false},
        {on: false, fail: false}
      ]
    }
  },
  watch: {
    steps() {
      this.clearIntervals()
      this.userMove = false
      if (!this.steps.length) {
        return
      }
      this.nextStep = 0
      this.showStep(0)
    }
  },
  methods: {
    beep(idx) {
      let note = "fail"
      switch (idx) {
        case 0: note = "do"; break;
        case 1: note = "re"; break;
        case 2: note = "mi"; break;
        case 3: note = "fa"; break;
      }
      const audio = new Audio(require(`@/assets/${note}.mp3`))
      audio.play()
    },
    onClickHandler(idx) {
      if (!this.userMove) {
        return
      }
      const rightMove = this.steps[this.nextStep]
      if (idx !== rightMove || rightMove === undefined) {
        clearInterval(this.intervals.success)
        clearInterval(this.intervals.prevStepLight)
        this.lights[idx].fail = true
        this.beep(-1)
        if (rightMove !== undefined) {
          this.lights[rightMove].on = true
        }
        this.$emit("fail")
        return
      }
      // Принудительно выключаем предыдущюю ячейку при клике
      if (this.prevStep !== null) {
        if (this.intervals.prevStepLight) {
          clearInterval(this.intervals.prevStepLight)
        }
        this.lights[this.prevStep].on = false
      }
      this.lights[idx].on = true
      this.beep(idx)
      this.prevStep = idx

      // Выключаем предыдущую ячейку по истечении времени
      this.intervals.prevStepLight = setTimeout(() => {
        this.lights[this.prevStep].on = false
      }, this.lightTime)

      this.nextStep++
      if (this.nextStep === this.steps.length) {
        this.intervals.success = setTimeout(() => {
          this.$emit("success")
          this.prevStep = null
        }, this.lightTime * 1.1)
      }
    },
    showStep(idx) {
      if (idx === this.steps.length) {
        this.userMove = true
        return
      }
      this.lights[this.steps[idx]].on = true
      this.beep(this.steps[idx])
      this.intervals.nextStepOff = setTimeout(() => this.lights[this.steps[idx]].on = false, this.lightTime)
      this.intervals.nextStepOn = setTimeout(() => this.showStep(idx + 1), this.lightTime * 1.1)
    },
    clearIntervals() {
      this.lights.forEach(light => {
        light.on = false
        light.fail = false
      })
      for (const interval of Object.values(this.intervals)) {
        if (interval) {
          clearInterval(interval)
        }
      }
    }
  }
}
</script>


<style scoped>
.simon-field {
  display: flex;
  flex-direction: row;
  width: 205px;
  min-width: 205px;
  flex-wrap: wrap;
}
</style>