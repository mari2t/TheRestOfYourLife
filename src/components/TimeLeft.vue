<template>
  <div class="question-container">
    <div class="input-container">
      <label for="birthday" class="question">1. 誕生日を入力してください:</label
      ><br />
      <label for="birthday">(Please enter your birthday:)</label><br />
      <input
        type="date"
        id="birthday"
        v-model="birthday"
        @input="updateBirthday($event.target.value)"
      />
      <p class="note-text">現在{{ age.years }} 歳？</p>
      <p class="note-text">(Are you {{ age.years }} years old?)</p>
    </div>
    <div class="input-container">
      <div>
        <label for="lifeExpectancy" class="question"
          >2. いつまで生きたいですか? :　</label
        ><br />
        <label for="birthday"
          >(Please enter how long you would like to live.)</label
        ><br />
        <input
          type="date"
          id="lifeExpectancy"
          v-model="selectedDate"
          :style="{ color: specialYearSelected === true ? 'white' : 'black' }"
          @input="updateLifeExpectancy($event.target.value)"
        />
      </div>
      <br />
      <div>or 年後(after x years)</div>
      <div>
        <button class="button-year" @click="addYearsToLifeExpectancy(10)">
          10
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(20)">
          20
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(40)">
          40
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(60)">
          60
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(80)">
          80
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(100)">
          100
        </button>
        <button class="button-year" @click="addYearsToLifeExpectancy(100059)">
          100059
        </button>
        <p class="note-text">{{ lifeUntilDead }} 歳まで生きる？</p>
        <p class="note-text">
          (Do you want to live until {{ lifeUntilDead }} years old?)
        </p>
      </div>
    </div>
    <div class="button-container">
      <button class="button-calculate" @click="calculateTimeLeft()">
        3. 残りの時間は？<br />(How much time <br />do you have left?)
      </button>
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
      deadDate: {},
      timeLeftToday: {},
      timeLeftThisWeek: {},
      timeLeftThisMonth: {},
      timeLeftThisYear: {},
      timeLeftThisAge: {},
      selectedDate: null,
      age: { years: null, months: null, months12: null },
      specialYearSelected: false, // 100059用
      SPECIAL_YEAR: 100059, // 1100059用
    };
  },
  methods: {
    //誕生日関数
    updateBirthday(birthday) {
      //アラート用
      const currentDate = dayjs().startOf("day");

      //　今日以前の誕生日の場合
      if (currentDate.isBefore(birthday)) {
        alert(
          "今日以前の誕生日を選択してください。(Select a birthday before today.)"
        );
        this.birthday = null;
        return;
      }
      this.birthday = birthday;
      this.calculateAge();
    },

    //いつまで生きたいか関数
    updateLifeExpectancy(lifeExpectancy) {
      this.lifeExpectancy = lifeExpectancy;
      const now = dayjs();
      const nowYear = now.year();
      const date = new Date();
      const thisMonth = String(date.getMonth() + 1).padStart(2, "0");
      const SPECIAL_DATE = "9999-12-31"; // 10万59歳用変数

      //10万59歳はカレンダー表記できないのでifで分岐
      if (this.specialYearSelected === true) {
        const birthYear = parseInt(this.birthday.slice(0, 4));
        const deadYear = parseInt(nowYear + this.SPECIAL_YEAR);
        const deadMonth = thisMonth;
        this.deadDate.year = deadYear;
        this.deadDate.month = deadMonth;
        this.lifeUntilDead = deadYear - birthYear;
      } else {
        const birth = moment(this.birthday, "YYYY-MM-DD");
        const dead = moment(this.lifeExpectancy, "YYYY-MM-DD");
        const years = dead.diff(birth, "year");
        this.deadDate.year = dead.year();
        this.deadDate.month = dead.month() + 1;
        this.lifeUntilDead = years;
      }
    },
    //現在の年齢関数
    calculateAge() {
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

    // xxx年後関数
    addYearsToLifeExpectancy(years) {
      const currentDate = new Date();
      let newYear;

      //誕生日が未入力の場合
      if (!this.birthday) {
        alert("誕生日を入力してください。(Please enter your birthday.)");
        return;
      }

      if (years === this.SPECIAL_YEAR) {
        this.selectedDate = "9999-12-31";
        this.specialYearSelected = true; // 特別な年を選択
      } else {
        newYear = currentDate.getFullYear() + years;
        currentDate.setFullYear(newYear);
        this.selectedDate = currentDate.toISOString().substring(0, 10);
        this.specialYearSelected = false; // 特別な年を選択しない
      }
      this.updateLifeExpectancy(this.selectedDate);
    },

    // 計算関数
    calculateTimeLeft() {
      const now = dayjs();
      const endOfDay = dayjs().endOf("day");
      const endOfWeek = dayjs().endOf("week");
      const endOfMonth = dayjs().endOf("month");
      const endOfYear = dayjs().endOf("year");
      const endOfLife = dayjs(this.lifeExpectancy);
      let yearsRemaining = 0;
      let monthsRemaining = 0;
      let daysRemaining = 0;

      //アラート用
      const currentDate = dayjs().startOf("day");
      const tomorrowDate = dayjs().add(1, "day").startOf("day");
      const inputDateBirthday = dayjs(this.birthday);
      const inputDateLifeExpectancy = dayjs(this.lifeExpectancy);
      if (!this.birthday) {
        alert("誕生日を入力してください。(Please enter your birthday.)");
        return;
      }
      if (!this.lifeExpectancy) {
        alert(
          "いつまで生きたいか入力してください。(Please enter how long you would like to live.)"
        );
        return;
      }
      if (currentDate.isBefore(inputDateBirthday)) {
        alert(
          "今日以前の誕生日を選択してください。(Select a birthday before today.)"
        );
        return;
      }
      if (inputDateLifeExpectancy.isBefore(tomorrowDate)) {
        alert(
          "明日以降の生きたい日付を選択してください。(Select the date you want to live after tomorrow.Die tomorrow live tonight!)"
        );
        return;
      }

      // 現在と寿命の差を計算
      //100059歳用条件分岐
      if (this.specialYearSelected === true) {
        yearsRemaining = this.SPECIAL_YEAR - 1;
        monthsRemaining = 11;
      } else {
        yearsRemaining = dayjs
          .duration(endOfLife.diff(now, "years"), "years")
          .asYears();
        monthsRemaining = dayjs
          .duration(endOfLife.diff(now, "months"), "months")
          .asMonths();
      }
      daysRemaining = endOfLife.diff(now, "day") + 1;

      if (yearsRemaining < 1) {
        yearsRemaining = 0;
        monthsRemaining = monthsRemaining;
      } else {
        if (endOfLife.$y === 9999) {
          monthsRemaining = monthsRemaining;
        } else {
          monthsRemaining = monthsRemaining - yearsRemaining * 12;
        }
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
        });
      (this.timeLeftThisMonth.percentage = Math.floor(
        (this.timeLeftThisMonth.days / 30) * 100
      )),
        // 今年の残り時間
        (this.timeLeftThisYear = {
          months: dayjs
            .duration(endOfYear.diff(now, "months"), "months")
            .asMonths(),
          weeks: dayjs
            .duration(endOfYear.diff(now, "weeks"), "weeks")
            .asWeeks(),
          percentage: Math.floor(
            (this.timeLeftThisYear.weeks / (24 * 365)) * 100
          ),
        });
      (this.timeLeftThisYear.percentage = Math.floor(
        (this.timeLeftThisYear.weeks / 52) * 100
      )),
        // 残りの人生

        (this.timeLeftThisAge = {
          years: yearsRemaining,
          months: monthsRemaining,
          percentage: LifeRemainingPercentage,
          age: this.lifeUntilDead,
          deadYear: this.deadDate.year,
          deadMonth: this.deadDate.month,
        });
      // propsとして渡す
      this.$emit("update-time-left-today", this.timeLeftToday);
      this.$emit("update-time-left-this-week", this.timeLeftThisWeek);
      this.$emit("update-time-left-this-month", this.timeLeftThisMonth);
      this.$emit("update-time-left-this-year", this.timeLeftThisYear);
      this.$emit("update-time-left-life", this.timeLeftThisAge);
    },
  },
};
</script>
<style scoped>
.question-container {
  padding: 2px;
  margin-top: 0.5rem;
  display: flex;
  justify-content: space-evenly;
}
.input-container {
  padding: 1rem;
  margin-right: 1%;
  margin-left: 1%;
  border-radius: 1rem;
  border: 0.5rem solid #d7d4d4;
  width: 90%;
  height: 16rem;
  background-color: white;
  color: black;
}
.button-container {
  padding: 1rem;
  margin-right: 1%;
  margin-left: 1%;
  text-align: center;
  justify-content: center;
  border-radius: 1rem;
  border: 0.5rem solid #d7d4d4;
  width: 90%;
  height: 16rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: white;
}
.question {
  font-weight: bold;
}
.note-text {
  color: gray;
}
.button-year {
  font-size: medium;
  padding: 0.2rem 0.4rem 0.2rem 0.4rem;
  margin: 2px;
  justify-content: space-evenly;
  border-radius: 0.5rem;
  color: #3d4041;
  background-color: #c3c5c6;
  border: none;
}
.button-year:hover {
  background-color: #ccd4d8;
}
.button-calculate {
  font-size: larger;
  font-weight: bold;
  padding: 2px;
  margin: 2px;
  border: none;
  border-radius: 1rem;
  width: 16rem;
  color: #ede9e9;
  background-color: #7e8081;
  border-bottom: 5px solid #454646;
}
.button-calculate:hover {
  color: #3d4041;
  margin-top: 3px;
  background-image: linear-gradient(to right, #50cc7f 0%, #f5d100 100%);
  border-bottom: 2px solid #376f80;
}
/* 100059選択時にカレンダー文字を白に設定 */
.button-year-special {
  color: #ffffff;
}
@media (max-width: 1200px) {
  .question-container {
    flex-direction: column;
    align-items: center;
  }
  .input-container,
  .button-container {
    width: 70%;
    margin-bottom: 1rem;
  }
}
@media (max-width: 550px) {
  .input-container {
    width: 100%;
    padding: 0.5rem;
    margin-right: 0;
    margin-left: 0;
    height: auto;
  }
}
</style>
