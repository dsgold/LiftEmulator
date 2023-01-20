<template>
  <div :class="{waiting: this.lift.status === 'waiting'}"
       :style="movingStyle"
       class="lift">
    <div v-if="this.lift.status === 'moving'" class="lift__container">
      <div class="lift__target-floor">
        {{ this.lift.floor_to_move }}
      </div>
      <div class="lift__direction"
      >
        <svg :style="this.lift.floor_to_move > this.lift.floor ? 'transform:rotate(180deg)' : ''"
             xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
             class="w-6 h-6">
          <path d="M19.5 13.5L12 21m0 0l-7.5-7.5M12 21V3"/>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EmulatorLift",
  props: {
    lift: {
      type: Object,
      required: true,
      default: {},
    },
  },
  computed: {
    movingStyle() {
      return {
        transform: this.lift.status === 'moving' ? `translateY(${(this.lift.floor - this.lift.floor_to_move) * 104}px)` : 'none',
        transition: this.lift.status === 'moving' ?  `transform ${Math.abs((this.lift.floor - this.lift.floor_to_move))}s ease-in-out` : 'none',
        bottom: `${this.lift.time ? (this.lift.floor - 1) * 104 + this.lift.time : (this.lift.floor - 1) * 104}px`,
      }
    },
  }
}
</script>

<style scoped>
  .lift {
    width: 50px;
    height: 85px;
    margin: 15px 20px 0 20px;
    background-color: grey;
  }

  .lift__container {
    display: flex;
    justify-content: center;
    background-color: #242424;
    border-radius: 10px;
    padding: 0 5px;
    margin: 5px;
    height: 20px;
  }

  .lift__target-floor {
    font-size: 12px;
  }

  .lift__direction {
    width: 12px;
    height: 12px;
  }

  @keyframes pulse {
    50% {
      opacity: 0;
    }
  }

  @-webkit-keyframes pulse {
    50% {
      opacity: 0;
    }
  }

  .waiting {
    animation: pulse 1s linear infinite;
    -webkit-animation: pulse 1s linear infinite;
  }
</style>