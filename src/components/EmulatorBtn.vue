<template>
  <div class="btn-container">
    <div class="btn" :class="{'btn-called': this.isCalled}">
    </div>
  </div>
</template>

<script>
export default {
  name: "EmulatorBtn",
  props: {
    isCalled: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  computed: {
  }
}
</script>

<style scoped>
.btn-container {
  position: relative;
  width: 20px;
  height: 20px;
  cursor: pointer;
  margin: 10px;
}
.btn {
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 2px solid grey;
}
.btn::before {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: gray;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}

.btn-called{
  border: 2px solid #747bff;
}
.btn-called::before {
  background-color: #747bff;
}
</style>