<template>
  <div>
    <p class="text">
      {{ remaining.days }} 日(days) （{{ remaining.hours }} 時間(hours)）
    </p>
    <p class="percent">{{ remaining.percentage }} %</p>
  </div>
</template>

<script>
export default {
  props: {
    remaining: {
      type: Object,
      default: () => ({ days: 0, hours: 0, percentage: 0 }),
    },
  },
};
</script>
<style scoped>
.text {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  color: black;
}
.percent {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  justify-content: space-around;
  font-weight: bold;
  font-size: larger;
  color: rgba(0, 0, 0, 0);
  text-shadow: 1px 1px 0 #9db;
  -webkit-text-stroke: 1px #606060;
}
</style>
