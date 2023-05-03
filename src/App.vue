<template>
  <div id="app">
    <h1>The Rest Of Your Life</h1>

    <TimeLeft
      :time-left-today="timeLeftToday"
      :time-left-this-week="timeLeftThisWeek"
      :time-left-this-month="timeLeftThisMonth"
      :time-left-this-year="timeLeftThisYear"
      :time-left-in-ten-years="timeLeftInTenYears"
      :seasons-left="seasonsLeft"
    />
  </div>
</template>

<script>
import TimeLeft from "./components/TimeLeft.vue";
import dayjs from "dayjs";
import duration from "dayjs/plugin/duration";

export default {
  components: {
    TimeLeft,
  },
  data() {
    return {
      birthday: null,
      lifeExpectancy: null,
      timeLeftToday: {},
      timeLeftThisWeek: {},
      timeLeftThisMonth: {},
      timeLeftThisYear: {},
      timeLeftInTenYears: {},
      seasonsLeft: null,
    };
  },
  methods: {
    calculateTimeLeft() {
      if (!this.birthday || !this.lifeExpectancy) {
        return;
      }

      const now = dayjs();
      const endOfDay = dayjs().endOf("day");
      const endOfWeek = dayjs().endOf("week");
      const endOfMonth = dayjs().endOf("month");
      const endOfYear = dayjs().endOf("year");
      const endOfTenYears = dayjs().add(10, "year").endOf("year");
      const endOfLife = dayjs(this.birthday).add(this.lifeExpectancy, "year");

      this.timeLeftToday = {
        seconds: dayjs
          .duration(endOfDay.diff(now, "seconds"), "seconds")
          .asSeconds(),
        minutes: dayjs
          .duration(endOfDay.diff(now, "minutes"), "minutes")
          .asMinutes(),
        hours: dayjs.duration(endOfDay.diff(now, "hours"), "hours").asHours(),
      };

      this.timeLeftThisWeek = {
        days: dayjs.duration(endOfWeek.diff(now, "days"), "days").asDays(),
        hours: dayjs.duration(endOfWeek.diff(now, "hours"), "hours").asHours(),
      };

      this.timeLeftThisMonth = {
        weeks: dayjs.duration(endOfMonth.diff(now, "weeks"), "weeks").asWeeks(),
        days: dayjs.duration(endOfMonth.diff(now, "days"), "days").asDays(),
        hours: dayjs.duration(endOfMonth.diff(now, "hours"), "hours").asHours(),
      };

      this.timeLeftThisYear = {
        months: dayjs
          .duration(endOfYear.diff(now, "months"), "months")
          .asMonths(),
        weeks: dayjs.duration(endOfYear.diff(now, "weeks"), "weeks").asWeeks(),
        days: dayjs.duration(endOfYear.diff(now, "days"), "days").asDays(),
        hours: dayjs.duration(endOfYear.diff(now, "hours"), "hours").asHours(),
      };

      this.timeLeftInTenYears = {
        years: dayjs
          .duration(endOfTenYears.diff(now, "years"), "years")
          .asYears(),
        months: dayjs
          .duration(endOfTenYears.diff(now, "months"), "months")
          .asMonths(),
        weeks: dayjs
          .duration(endOfTenYears.diff(now, "weeks"), "weeks")
          .asWeeks(),
        days: dayjs.duration(endOfTenYears.diff(now, "days"), "days").asDays(),
      };

      const seasonsPerYear = 4;
      const yearsLeft = dayjs
        .duration(endOfLife.diff(now, "years"), "years")
        .asYears();
      this.seasonsLeft = Math.floor(yearsLeft * seasonsPerYear);
    },
  },
};
</script>
