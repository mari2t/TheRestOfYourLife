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
  border-radius: 10px;
}

.bar {
  height: 100%;
  position: absolute;
  bottom: 0;
  transition: width 1s ease-in-out;
}

.bg-white {
  background-image: linear-gradient(to right, #3d9d6261 0%, #c8b75876 100%);
  z-index: 2;
  border-radius: 10px;
}

.bg-green {
  background-color: #f3f6f6;
  width: 100%;
  z-index: 1;
  border-radius: 10px;
}
</style>
