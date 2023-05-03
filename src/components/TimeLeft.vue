<template>
  <div>
    <div>
      <label for="birthday">誕生日:</label>
      <input
        type="date"
        id="birthday"
        @input="updateBirthday($event.target.value)"
      />
    </div>
    <div>
      <label for="lifeExpectancy">何年まで生きたいか:</label>
      <input
        type="date"
        id="lifeExpectancy"
        v-model="selectedDate"
        @input="updateLifeExpectancy($event.target.value)"
      />
      <button @click="addYearsToLifeExpectancy(20)">20年後</button>
      <button @click="addYearsToLifeExpectancy(40)">40年後</button>
      <button @click="addYearsToLifeExpectancy(80)">80年後</button>
      <button @click="addYearsToLifeExpectancy(100)">100年後</button>
      <button @click="addYearsToLifeExpectancy(200)">200年後</button>
      <button @click="addYearsToLifeExpectancy(1000)">10059年後</button>
    </div>
    <div>
      <button @click="calculateTimeLeft()">計算</button>
    </div>
    <h3>
      年齢: {{ age.years }}歳{{ age.months12 }}か月（生後{{ age.months }}か月）
    </h3>

    <h3>あと: {{ age.years }}年{{ age.months12 }}か月</h3>
    <div>
      <h3>今日が終わるまでの秒、分、時間</h3>
      <p>{{ timeLeftToday.seconds }} 秒</p>
      <p>{{ timeLeftToday.minutes }} 分</p>
      <p>{{ timeLeftToday.hours }} 時間</p>
    </div>
    <div>
      <h3>今週が終わるまでの時間、日</h3>
      <p>{{ timeLeftThisWeek.days }} 日</p>
      <p>{{ timeLeftThisWeek.hours }} 時間</p>
    </div>
    <div>
      <h3>今月が終わるまでの時間、日、週</h3>
      <p>{{ timeLeftThisMonth.weeks }} 週</p>
      <p>{{ timeLeftThisMonth.days }} 日</p>
      <p>{{ timeLeftThisMonth.hours }} 時間</p>
    </div>
    <div>
      <h3>今年が終わるまでの時間、日、週、月</h3>
      <p>{{ timeLeftThisYear.months }} 月</p>
      <p>{{ timeLeftThisYear.weeks }} 週</p>
      <p>{{ timeLeftThisYear.days }} 日</p>
      <p>{{ timeLeftThisYear.hours }} 時間</p>
    </div>
    <div>
      <h3>寿命が来るまでの春、夏、秋、冬の回数</h3>
      <p>{{ seasonsLeft }} 回</p>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
import duration from "dayjs/plugin/duration";

dayjs.extend(duration);

export default {
  data() {
    return {
      birthday: null,
      lifeExpectancy: null,
      timeLeftToday: {},
      timeLeftThisWeek: {},
      timeLeftThisMonth: {},
      timeLeftThisYear: {},
      selectedDate: null,
      seasonsLeft: null,
      age: { years: 0, months: 0, months12: 0 },
    };
  },
  methods: {
    //誕生日関数
    updateBirthday(birthday) {
      this.birthday = birthday;
      this.calculateAge();
    },
    //いつまで生きたいか関数
    updateLifeExpectancy(lifeExpectancy) {
      this.lifeExpectancy = lifeExpectancy;
    },
    //現在の年齢関数
    calculateAge() {
      if (!this.birthday) {
        return;
      }
      const now = dayjs();
      const birth = dayjs(this.birthday);
      const years = now.diff(birth, "year");
      // 年齢に合わせて今年の誕生日を計算する
      birth.add(years, "years");
      const months = now.diff(birth, "month");
      const months12 = months - years * 12;
      this.age = { years, months, months12 };
    },
    // xxx年後を追加関数
    addYearsToLifeExpectancy(years) {
      if (!this.birthday) {
        alert("誕生日を入力してください。");
        return;
      }
      const currentDate = new Date();
      currentDate.setFullYear(currentDate.getFullYear() + years);
      this.selectedDate = currentDate.toISOString().substring(0, 10);
      // 余命への反映
      const lifeExpectancy = dayjs(this.birthday)
        .add(years, "year")
        .diff(dayjs(), "year");
      this.updateLifeExpectancy(lifeExpectancy);
    },
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

      const seasonsPerYear = 4;
      const yearsLeft = dayjs
        .duration(endOfLife.diff(now, "years"), "years")
        .asYears();
      this.seasonsLeft = Math.floor(yearsLeft * seasonsPerYear);
    },
  },
};
</script>
