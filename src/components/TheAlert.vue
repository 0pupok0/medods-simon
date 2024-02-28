<template>
<div :class="['the-alert', {'the-alert_fail': variant === 'fail', 'the-alert_success': variant === 'success'}]">
  {{text}}
</div>
</template>

<script>
export default {
  name: "TheAlert",
  props: {
    text: {
      type: String,
      default: ""
    },
    // success - успешно
    // fail - ошибка
    variant: {
      type: String,
      default: "Success"
    },
    secondsCount: {
      type: Number,
      default: 10
    }
  },
  data() {
    return {
      timer: 0,
      interval: null
    }
  },
  created() {
    this.timer = this.secondsCount
    console.log(this.timer)
    if (this.timer) {
      this.interval = setInterval(() => this.timer--, 1000)
    }
  },
  beforeDestroy() {
    if (this.interval) {
      clearInterval(this.interval)
    }
  },
  watch: {
    timer() {
      if (!this.timer) {
        this.$emit("hide")
        if (this.interval) {
          clearInterval(this.interval)
        }
      }
    }
  }
}
</script>


<style scoped>
.the-alert {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 100;
  font-size: .9em;
  border: 1px solid #43799b;
  border-radius: 10px;
  padding: 10px;
}

.the-alert_success {
  background-color: #94e5c6;
}

.the-alert_fail {
  background-color: #fcb4b4;
}
</style>