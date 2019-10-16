<template>
  <div></div>
</template>

<script>
import PersianDate from "persian-date";

export default {
  data() {
    return {
      MonthArray: []
    };
  },
  name: "PersianDateRangePicker",
  created() {
    this.generateMonthArray();
  },
  methods: {
    generateMonthArray() {
      let currentDate = new PersianDate();
      let currentDateArray = [];
      let firstDayOfMonth = [];
      currentDateArray[2] = this.fixPersianNumber(currentDate.format("D"));
      currentDateArray[1] = this.fixPersianNumber(currentDate.format("M")) - 1;
      currentDateArray[0] = this.fixPersianNumber(currentDate.format("YYYY"));

      firstDayOfMonth = [currentDateArray[0], currentDateArray[1], "1"];
      this.makeDaysOfMonth(firstDayOfMonth);

    },
    fixPersianNumber(str) {
      const persianNumberArr = [
        /۰/g,
        /۱/g,
        /۲/g,
        /۳/g,
        /۴/g,
        /۵/g,
        /۶/g,
        /۷/g,
        /۸/g,
        /۹/g
      ];
      const arabicNumberArr = [
        /٠/g,
        /١/g,
        /٢/g,
        /٣/g,
        /٤/g,
        /٥/g,
        /٦/g,
        /٧/g,
        /٨/g,
        /٩/g
      ];
      if (typeof str === "string") {
        let i = 0;
        for (; i < 10; i++) {
          str = str
            .replace(persianNumberArr[i], i)
            .replace(arabicNumberArr[i], i);
        }
      }
      return str;
    },
    makeDaysOfMonth(startDate) {
      const isLeapYear = new PersianDate(startDate).isLeapYear();
      let daysOfMonth = this.getDaysOfMonth(startDate[1], isLeapYear);

      for (let i = 1; i <= daysOfMonth; i++) {
        let dateObject = {};
        dateObject.isHoliday = false;
        dateObject.date = [startDate[0], startDate[1], i.toString()];
        dateObject.dayOfWeek = this.fixPersianNumber(new PersianDate(dateObject.date).format('d'));
        this.MonthArray.push(dateObject);
        debugger;
      }
      debugger;

    },
    getDaysOfMonth(month, isLeapYear) {
      if (month < 7) {
        return 31;
      } else {
        if (month === "12") {
          if (isLeapYear) {
            return 30;
          } else {
            return 29;
          }
        } else {
          return 30;
        }
      }
    }
  }
};
</script>

<style scoped></style>
