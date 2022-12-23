<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />

  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("Running handle before start,", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );

      const secondCards = [...firstCards];

      const cards = [...firstCards, ...secondCards];

      console.log(cards);
      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));

      this.settings.startedAt = new Date().getTime();
      console.log(this.settings.cardsContext);
      this.statusMatch = "match";
    },
    onGetResult() {
      //Get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //Switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>

