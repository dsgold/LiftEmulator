<template>
  <EmulatorConfigurator @add-floor="addFloor"
                        @add-lift="addLift"
                        @remove-floor="removeFloor"
                        @remove-lift="removeLift"/>
  <EmulatorMain :floors="floors" :lifts="lifts"/>
</template>

<script>
import EmulatorConfigurator from "./components/EmulatorConfigurator.vue";
import EmulatorMain from "./components/EmulatorMain.vue";

export default {
  components: {EmulatorMain, EmulatorConfigurator},
  data() {
    return {
      floors: [],
      lifts: [],
    }
  },
  methods: {
    addFloor() {
      this.floors.push(this.floors.length + 1)
    },
    addLift() {
      const newLift = {
        shaft: this.lifts.length + 1,
        floor: 1,
        status: 'free',
        time: 0,
        start: 0,
      };
      this.lifts.push(newLift)
    },
    removeFloor() {
      this.floors.pop();
    },
    removeLift() {
      this.lifts.pop();
    },
    parseLocalStorage(str, _default) {
      return localStorage.getItem(str) ?
          JSON.parse(localStorage.getItem(str)) :
          _default;
    }
  },
  mounted() {
    this.lifts = this.parseLocalStorage('liftList', [{
      shaft: this.lifts.length + 1,
      floor: 1,
      status: 'free',
      time: 0,
      start: 0,
    }]);
    this.floors = this.parseLocalStorage('floorsList', [1, 2, 3, 4, 5]);
  },
}
</script>

<style scoped>
</style>
