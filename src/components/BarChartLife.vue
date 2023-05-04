<template>
  <div class="bar-chart">
    <div class="bar-container">
      <div class="bar bg-green" :style="{ width: 100 + '%' }"></div>
      <div class="bar bg-white" :style="{ width: barWidth + '%' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    timeLeft: {
      type: Number,
      required: true,
    },
  },

  computed: {
    barWidth() {
      if (this.timeLeft) {
        const percentage = this.timeLeft;
        return percentage;
      }
      return "0%";
    },
  },
};
</script>

<style scoped>
.bar-chart {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  width: 100%;
  height: 40px;
}

.bar-container {
  height: 100%;
  width: 80%;
  position: relative;
  border: 1px solid dimgray;
  border-radius: 2rem;
}

.bar {
  height: 100%;
  position: absolute;
  bottom: 0;
  transition: width 1s ease-in-out;
  border-radius: 2rem;
}

.bg-white {
  background-image: linear-gradient(to right, #50cc7f 0%, #f5d100 100%);
  z-index: 2;
  left: 0;
}

.bg-green {
  background-color: #f3f6f6;
  width: 100%;
  z-index: 1;
}
</style>
