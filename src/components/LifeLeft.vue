<template>
  <div>
    <p class="text">
      {{ timeLeftLife.years }} 年 {{ timeLeftLife.months }} か月　({{
        timeLeftLife.years
      }}
      years {{ timeLeftLife.months }} months)
    </p>
    <p class="text">
      {{ timeLeftLife.deadYear }} 年 {{ timeLeftLife.deadMonth }} 月　まで (
      Until {{ timeLeftLife.deadMonth }} / {{ timeLeftLife.deadYear }})
    </p>
    <p class="percent">{{ timeLeftLife.percentage }} %</p>
  </div>
</template>

<script>
export default {
  props: {
    age: {
      type: Object,
      default: () => ({ years: 0, months: 0, months12: 0 }),
    },
    timeLeftLife: {
      type: Object,
      default: () => ({
        years: 0,
        months: 0,
        percentage: 0,
        age: 0,
        deadYear: 0,
        deadMonth: 0,
      }),
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
