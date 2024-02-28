<template>
  <div id="app" class="app">
    <simon-field :steps="steps" :light-time="lightTime" @success="addStep" @fail="failShow = false; gameIsOn = false"/>
    <div class="app__controls">
      <div class="app__label">Раунд: {{counter}}</div>
      <button v-if="!gameIsOn" class="app__start-btn" @click="start">
        Старт
      </button>
      <button v-if="gameIsOn" class="app__start-btn" @click="stop">
        Стоп
      </button>
      <div class="app__controls_difficulties">
        <div class="app__label">Уровень сложности</div>
        <div class="app__controls_difficulty">
          <input v-model="difficulty[0]" :disabled="gameIsOn" type="checkbox" @input="setDiff(0)"> Легкий
        </div>
        <div class="app__controls_difficulty">
          <input v-model="difficulty[1]" :disabled="gameIsOn" type="checkbox" @input="setDiff(1)"> Средний
        </div>
        <div class="app__controls_difficulty">
          <input v-model="difficulty[2]" :disabled="gameIsOn" type="checkbox" @input="setDiff(2)"> Сложный
        </div>
      </div>
    </div>
    <the-alert
      v-if="failShow"
      text="Вы проиграли"
      variant="fail"
      @hide="failShow = false"
    />
  </div>
</template>

<script>

import SimonField from "@/components/SimonField.vue";
import TheAlert from "@/components/TheAlert.vue";

export default {
  name: 'App',
  components: {TheAlert, SimonField},
  data() {
    return {
      steps: [],
      failShow: false,
      counter: 0,
      difficulty: [false, true, false],
      lightTime: 1000,
      gameIsOn: false
    }
  },
  methods: {
    addStep() {
      this.counter++
      this.steps.push(~~(Math.random() * 4))
    },
    start() {
      this.gameIsOn = true
      this.failShow = false
      this.counter = 1
      this.steps = [~~(Math.random() * 4)]
    },
    stop() {
      this.gameIsOn = false
      this.failShow = false
      this.counter = 1
      this.steps = []
    },
    setDiff(diff) {
      switch (diff) {
        case 0: this.lightTime = 1500; break;
        case 1: this.lightTime = 1000; break;
        case 2: this.lightTime = 400;  break;
      }
      this.difficulty.forEach((_, idx) => this.difficulty[idx] = diff === idx)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 100px;
  padding-top: 25vh;
}

.app__controls {
  height: 100%;
}

.app__label {
  font-size: 18px;
  font-weight: bold;
}

.app__controls_difficulties {
  display: flex;
  flex-direction: column;
}

.app__controls_difficulty {
  display: flex;
  flex-direction: row;
}

.app__start-btn {
  background-color: #6DABE8;
  border: 0;
  border-radius: 10px;
  padding: 10px 0;
  width: 110px;
  height: fit-content;
}
</style>
