<template>
  <div id="app">
    <h1>The Rest Of Your Life</h1>
    <TimeLeft
      @update-time-left-today="updateTimeLeft('today', $event)"
      @update-time-left-this-week="updateTimeLeft('thisWeek', $event)"
      @update-time-left-this-month="updateTimeLeft('thisMonth', $event)"
      @update-time-left-this-year="updateTimeLeft('thisYear', $event)"
      @update-age="updateAge"
      @update-time-left-life="updateTimeLeftLife"
    />

    <LifeLeft :age="age" :timeLeftLife="timeLeftLife" />
    <BarChart :time-left="timeLeftLife.percentage" />
    <p>今日の残り</p>
    <BarChart :time-left="timeLeft.today" />
    <p>今週の残り</p>
    <BarChart :time-left="timeLeft.thisWeek" />
    <p>今月の残り</p>
    <BarChart :time-left="timeLeft.thisMonth" />
    <p>今年の残り</p>
    <BarChart :time-left="timeLeft.thisYear" />
  </div>
</template>

<script>
import TimeLeft from "./components/TimeLeft.vue";
import BarChart from "./components/BarChart.vue";
import LifeLeft from "./components/LifeLeft.vue";

export default {
  components: {
    TimeLeft,
    BarChart,
    LifeLeft,
  },
  data() {
    return {
      timeLeft: {
        today: 0,
        thisWeek: 0,
        thisMonth: 0,
        thisYear: 0,
      },
      age: { years: 0, months: 0, months12: 0 },
      timeLeftLife: { years: 0, months: 0, percentage: 0 },
    };
  },
  methods: {
    updateTimeLeft(key, value) {
      this.timeLeft[key] = value.percentage;
    },
    updateAge(age) {
      this.age = age;
    },
    updateTimeLeftLife(timeLeftLife) {
      this.timeLeftLife = timeLeftLife;
    },
  },
};
</script>
