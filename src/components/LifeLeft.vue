<template>
  <div>
    <h3>
      現在の年齢: {{ age.years }}歳{{ age.months12 }}か月（生後{{
        age.months
      }}か月）
    </h3>

    <h3>
      残りの人生あと: {{ timeLeftLife.years }} 年　{{
        timeLeftLife.months
      }}
      か月　{{ timeLeftLife.percentage }} パーセント
    </h3>
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
      default: () => ({ years: 0, months: 0, percentage: 0 }),
    },
  },
};
</script>
