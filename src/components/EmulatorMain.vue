<template>
  <div class="floors">
    <EmulatorLift
      v-for="(lift, index) in lifts"
      :key="index"
      :lift="lift"
      :style="{position:'absolute', left: `${lift.shaft * 90}px`}"
    />
    <div class="floor" v-for="(floor,index) in floors" :key="index">
      <EmulatorBtn
        @click="liftCall(floor)"
        :isCalled="!!this.calls.find(c=>c.floor === floor)"
      />
    </div>
  </div>
</template>

<script>
import EmulatorLift from './EmulatorLift.vue';
import EmulatorBtn from './EmulatorBtn.vue';

export default {
  name: 'EmulatorMain',
  components: { EmulatorBtn, EmulatorLift },
  props: {
    floors: {
      type: Array,
      required: true,
      default: [1, 2, 3, 4, 5],
    },
    lifts: {
      type: Array,
      required: true,
      default: [],
    },
  },
  data() {
    return {
      calls: [],
    }
  },
  methods: {
    liftCall(floor) {
      if (this.lifts.find(l => [l.floor, l.floor_to_move].includes(floor)) ||
        this.calls.find(c => c.floor === floor)) {
        return;
      }
      this.calls = [
        ...this.calls,
        {
          floor:floor,
          isCalled: false,
        },
      ];
    },
    liftMove(lift, floor) {
      lift.status = 'moving';
      lift.floor_to_move = floor;
      lift.start = new Date();
      setTimeout(() => {
        lift.start = 0;
        lift.time = 0;
        this.liftWait(lift, lift.floor_to_move)
      }, (Math.abs(lift.floor - lift.floor_to_move) * 1000) - lift.time)
    },
    liftWait(lift, floor) {
      lift.status = 'waiting';
      lift.floor = floor;
      lift.start = new Date();
      setTimeout(() => {
        lift.start = 0;
        lift.time = 0;
        lift.status = 'free';
        this.calls = this.calls.filter(fl => fl.floor !== floor);
      }, 3000 - lift.time)
    },
  },
  computed: {
    freeLifts() {
      return this.lifts.filter(({ status }) => status === 'free')
    },
  },
  watch: {
    calls(newVal) {
      const nextFloorCall = newVal.find(c => !c.isCalled)
      if (nextFloorCall && this.freeLifts.length) {
        const closestLift = this.freeLifts.reduce((p, n) =>
          Math.abs(p.floor - nextFloorCall.floor) < Math.abs(n.floor - nextFloorCall.floor) ?p : n
        )
        this.liftMove(closestLift, nextFloorCall.floor);
        nextFloorCall.isCalled = true;
      }
      localStorage.setItem('callList', JSON.stringify(this.calls));
    },
    lifts: {
      handler() {
        localStorage.setItem('liftList', JSON.stringify(this.lifts.map(lift =>
          ({ ...lift, time: lift.start ? new Date() - lift.start : 0 })
        )));
      },
      deep: true
    },
    floors: {
      handler() {
        localStorage.setItem('floorsList', JSON.stringify(this.floors));
      },
      deep: true
    },
  },
  mounted() {
    this.calls = localStorage.getItem('callList') ?
      JSON.parse(localStorage.getItem('callList')) :
      [];
    this.$nextTick(() => {
      this.lifts.forEach(lift => {
        if (lift.status === 'moving') {
          this.liftMove(lift, lift.floor_to_move);
        }
        if (lift.status === 'waiting') {
          this.liftWait(lift, lift.floor_to_move);
        }
      })
    });
  }

}
</script>

<style scoped>
  .floors {
    display: flex;
    flex-direction: column-reverse;
    position: relative;
  }

  .floor {
    width: 100vw;
    height: 100px;
    border-bottom: 4px solid grey;
    display: flex;
    flex-direction: row;
    justify-content: start;
    align-items: center;
  }
</style>