<template>
  <div>
    <p class="text">
      {{ remaining.months }} ヵ月(months)（{{ remaining.weeks }} 週(weeks)）
    </p>
    <p class="percent">{{ remaining.percentage }} %</p>
  </div>
</template>

<script>
export default {
  props: {
    remaining: {
      type: Object,
      default: () => ({ months: 0, weeks: 0, percentage: 0 }),
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
