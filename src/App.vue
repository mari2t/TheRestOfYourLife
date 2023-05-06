<template>
  <div id="app">
    <h1>The Rest Of Your Life</h1>
    <TimeLeft
      @update-time-left-today="updateTodayLeft($event)"
      @update-time-left-this-week="updateThisWeekLeft($event)"
      @update-time-left-this-month="updateThisMonthLeft($event)"
      @update-time-left-this-year="updateThisYearLeft($event)"
      @update-age="updateAge"
      @update-time-left-life="updateTimeLeftLife"
    />
    <div class="components-container">
      <div class="components-life">
        <span class="title">残りの人生(The Rest Of Your Life)</span>
        <LifeLeft :age="age" :timeLeftLife="timeLeftLife" />
        <BarChartLife :time-left="timeLeftLife.percentage" />
      </div>
    </div>
    <div class="components-container">
      <div class="components-display">
        <span class="title">今日の残り時間</span>
        <TodayLeft :remaining="todayLeft" />
        <BarChart :time-left="todayLeft.percentage" />
      </div>
      <div class="components-display">
        <span class="title">今週の残り時間</span>
        <ThisWeekLeft :remaining="thisWeekLeft" />
        <BarChart :time-left="thisWeekLeft.percentage" />
      </div>
      <div class="components-display">
        <span class="title">今月の残り時間</span>
        <ThisMonthLeft :remaining="thisMonthLeft" />
        <BarChart :time-left="thisMonthLeft.percentage" />
      </div>
      <div class="components-display">
        <span class="title">今年の残り時間</span>
        <ThisYearLeft :remaining="thisYearLeft" />
        <BarChart :time-left="thisYearLeft.percentage" />
      </div>
    </div>
    <div>
      <h2>Thi is the beginning of the rest of your life !!!</h2>
    </div>
  </div>
</template>

<script>
import TimeLeft from "./components/TimeLeft.vue";
import BarChart from "./components/BarChart.vue";
import BarChartLife from "./components/BarChartLife.vue";
import LifeLeft from "./components/LifeLeft.vue";
import TodayLeft from "./components/TodayLeft.vue";
import ThisWeekLeft from "./components/ThisWeekLeft.vue";
import ThisMonthLeft from "./components/ThisMonthLeft.vue";
import ThisYearLeft from "./components/ThisYearLeft.vue";

export default {
  components: {
    TimeLeft,
    BarChart,
    BarChartLife,
    LifeLeft,
    TodayLeft,
    ThisWeekLeft,
    ThisMonthLeft,
    ThisYearLeft,
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
      timeLeftLife: {
        years: 0,
        months: 0,
        percentage: 0,
        age: 0,
        deadYear: 0,
        deadMonth: 0,
      },
      todayLeft: {
        minutes: 0,
        hours: 0,
        percentage: 0,
      },
      thisWeekLeft: {
        days: 0,
        hours: 0,
        percentage: 0,
      },
      thisMonthLeft: {
        days: 0,
        weeks: 0,
        percentage: 0,
      },
      thisYearLeft: {
        months: 0,
        weeks: 0,
        percentage: 0,
      },
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
    updateTodayLeft(value) {
      this.todayLeft = value;
    },
    updateThisWeekLeft(value) {
      this.thisWeekLeft = value;
    },
    updateThisMonthLeft(value) {
      this.thisMonthLeft = value;
    },
    updateThisYearLeft(value) {
      this.thisYearLeft = value;
    },
  },
};
</script>
<style scoped>
#app {
  background-image: linear-gradient(60deg, #abecd6 0%, #fbed96 100%);
  min-height: 100vh;
}
h1 {
  text-align: center;
  font-weight: bold;
  color: antiquewhite;
  background-color: #4a3f00;
}
h2 {
  text-align: center;
}
.title {
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  width: 100%;
  font-weight: bolder;
  color: black;
}
.components-container {
  display: flex;
  justify-content: space-evenly;
  margin-top: 0.4rem;
  margin-right: 0.2rem;
  margin-left: 0.2rem;
}
.components-life {
  margin-right: 0.2rem;
  margin-left: 0.2rem;
  border-radius: 1rem;
  border: 0.5rem solid #d7d4d4;
  width: 60%;
  padding-top: 1rem;
  padding-bottom: 1rem;
  background-color: white;
}
.components-display {
  border-radius: 1rem;
  border: 0.5rem solid #d7d4d4;
  width: 30rem;
  padding-top: 0.4rem;
  padding-bottom: 1rem;
  margin-right: 0.2rem;
  margin-left: 0.2rem;
  background-color: white;
}
</style>
