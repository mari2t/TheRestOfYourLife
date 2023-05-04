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
      <label for="lifeExpectancy">いつまで生きたいですか:</label>
      <input
        type="date"
        id="lifeExpectancy"
        v-model="selectedDate"
        @input="updateLifeExpectancy($event.target.value)"
      />
      <button @click="addYearsToLifeExpectancy(1)">1年後</button>
      <button @click="addYearsToLifeExpectancy(3)">3年後</button>
      <button @click="addYearsToLifeExpectancy(10)">10年後</button>
      <button @click="addYearsToLifeExpectancy(20)">20年後</button>
      <button @click="addYearsToLifeExpectancy(40)">40年後</button>
      <button @click="addYearsToLifeExpectancy(60)">60年後</button>
      <button @click="addYearsToLifeExpectancy(80)">80年後</button>
      <button @click="addYearsToLifeExpectancy(100)">100年後</button>
      <button @click="addYearsToLifeExpectancy(200)">200年後</button>
      <button @click="addYearsToLifeExpectancy(100059)">10万59年後</button>
    </div>
    <p>{{ lifeUntilDead }}</p>
    <div>
      <button @click="calculateTimeLeft()">生きる</button>
    </div>
    <div>
      <h3>今日が終わるまでの秒、分、時間</h3>
      <p>{{ timeLeftToday.minutes }} 分</p>
      <p>{{ timeLeftToday.hours }} 時間</p>
      <p>{{ timeLeftToday.percentage }} パーセント</p>
    </div>
    <div>
      <h3>今週が終わるまでの時間、日</h3>
      <p>{{ timeLeftThisWeek.days }} 日</p>
      <p>{{ timeLeftThisWeek.hours }} 時間</p>
      <p>{{ timeLeftThisWeek.percentage }} パーセント</p>
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
  </div>
</template>

<script>
import dayjs from "dayjs";
import duration from "dayjs/plugin/duration";
import moment from "moment";

dayjs.extend(duration);

export default {
  data() {
    return {
      birthday: null,
      lifeExpectancy: null,
      lifeUntilDead: null,
      timeLeftToday: {},
      timeLeftThisWeek: {},
      timeLeftThisMonth: {},
      timeLeftThisYear: {},
      timeLeftThisAge: {},
      selectedDate: null,
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
      console.log(this.lifeExpectancy);

      if (this.lifeExpectancy === "+102082-05") {
        const now = moment();
        const nowYear = now.year();
        const birth = parseInt(this.birthday.slice(0, 4));
        const dead = 100059;
        const years = dead + (nowYear - birth);
        this.lifeUntilDead = `（${years} 歳まで生きる？）`;
      } else {
        const birth = moment(this.birthday, "YYYY-MM-DD");
        const dead = moment(this.lifeExpectancy, "YYYY-MM-DD");
        const years = dead.diff(birth, "year");
        this.lifeUntilDead = `（${years} 歳まで生きる？）`;
      }
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
      // プロップスとして渡す
      this.$emit("update-age", this.age);
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
      this.updateLifeExpectancy(this.selectedDate);
    },
    // 計算関数
    calculateTimeLeft() {
      if (!this.birthday || !this.lifeExpectancy) {
        return;
      }
      const now = dayjs();
      const endOfDay = dayjs().endOf("day");
      const endOfWeek = dayjs().endOf("week");
      const endOfMonth = dayjs().endOf("month");
      const endOfYear = dayjs().endOf("year");
      const endOfLife = dayjs(this.lifeExpectancy);

      // 余命１年未満の場合は１年未満と表示
      let yearsRemaining = dayjs
        .duration(endOfLife.diff(now, "years"), "years")
        .asYears();
      let monthsRemaining = dayjs
        .duration(endOfLife.diff(now, "months"), "months")
        .asMonths();
      let daysRemaining = dayjs
        .duration(endOfLife.diff(now, "days"), "days")
        .asDays();

      if (yearsRemaining < 1) {
        yearsRemaining = 0;
        monthsRemaining = monthsRemaining;
      } else {
        monthsRemaining = monthsRemaining - yearsRemaining * 12;
      }
      const daysSinceBirthday = now.diff(this.birthday, "day");
      const totalDaysRemaining = daysRemaining + daysSinceBirthday;
      const LifeRemainingPercentage = Math.floor(
        (daysRemaining / totalDaysRemaining) * 100
      );

      // 今日の残り時間
      this.timeLeftToday = {
        minutes: dayjs
          .duration(endOfDay.diff(now, "minutes"), "minutes")
          .asMinutes(),
        hours: dayjs.duration(endOfDay.diff(now, "hours"), "hours").asHours(),
      };
      this.timeLeftToday.percentage = Math.floor(
        (this.timeLeftToday.hours / 24) * 100
      );

      // 今週の残り時間
      this.timeLeftThisWeek = {
        days: dayjs.duration(endOfWeek.diff(now, "days"), "days").asDays(),
        hours: dayjs.duration(endOfWeek.diff(now, "hours"), "hours").asHours(),
      };
      (this.timeLeftThisWeek.percentage = Math.floor(
        (this.timeLeftThisWeek.hours / (24 * 7)) * 100
      )),
        // 今月の残り時間
        (this.timeLeftThisMonth = {
          weeks: dayjs
            .duration(endOfMonth.diff(now, "weeks"), "weeks")
            .asWeeks(),
          days: dayjs.duration(endOfMonth.diff(now, "days"), "days").asDays(),
          hours: dayjs
            .duration(endOfMonth.diff(now, "hours"), "hours")
            .asHours(),
        });
      (this.timeLeftThisMonth.percentage = Math.floor(
        (this.timeLeftThisMonth.hours / (24 * 30)) * 100
      )),
        // 今年の残り時間
        (this.timeLeftThisYear = {
          months: dayjs
            .duration(endOfYear.diff(now, "months"), "months")
            .asMonths(),
          weeks: dayjs
            .duration(endOfYear.diff(now, "weeks"), "weeks")
            .asWeeks(),
          days: dayjs.duration(endOfYear.diff(now, "days"), "days").asDays(),
          hours: dayjs
            .duration(endOfYear.diff(now, "hours"), "hours")
            .asHours(),
          percentage: Math.floor(
            (this.timeLeftThisYear.hours / (24 * 365)) * 100
          ),
        });
      (this.timeLeftThisYear.percentage = Math.floor(
        (this.timeLeftThisYear.hours / (24 * 365)) * 100
      )),
        // 残りの人生
        (this.timeLeftThisAge = {
          years: yearsRemaining,
          months: monthsRemaining,
          percentage: LifeRemainingPercentage,
        });
      // プロップスとして渡す
      this.$emit("update-time-left-today", this.timeLeftToday);
      this.$emit("update-time-left-this-week", this.timeLeftThisWeek);
      this.$emit("update-time-left-this-month", this.timeLeftThisMonth);
      this.$emit("update-time-left-this-year", this.timeLeftThisYear);
      this.$emit("update-time-left-life", this.timeLeftThisAge);
    },
  },
};
</script>
