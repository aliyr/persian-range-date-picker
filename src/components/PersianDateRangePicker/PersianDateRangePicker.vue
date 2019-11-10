<template>
  <div></div>
</template>

<script>
import PersianDate from "persian-date";

export default {
  data() {
    return {
      datetimePickerObj: {
        year: 1398,
        monthsArray: {
          firstMonth: {
            month: [],
            days: []
          },
          secondMonth: {
            month: [],
            days: []
          }
        }
      },
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
      let firstDayOfNextMonth = [];

      // make current date in an array
      currentDateArray[2] = parseInt(this.fixPersianNumber(currentDate.format("D")));
      currentDateArray[1] = parseInt(this.fixPersianNumber(currentDate.format("M")));
      currentDateArray[0] = parseInt(this.fixPersianNumber(currentDate.format("YYYY")));

      // store current year in the 'datetimePickerObj.year'
      this.datetimePickerObj.year = currentDateArray[0];

      // store current month and next month
      firstDayOfMonth = [currentDateArray[0], currentDateArray[1], 1];

      if (currentDateArray[1] === 12) {
        firstDayOfNextMonth = [currentDateArray[0] + 1, 1 , 1];
      } else{
        firstDayOfNextMonth = [currentDateArray[0], currentDateArray[1] + 1, 1];
      }


      this.datetimePickerObj.monthsArray.firstMonth.days = this.makeDaysOfMonth(firstDayOfMonth);
      this.datetimePickerObj.monthsArray.firstMonth.month = [currentDateArray[0], currentDateArray[1]];
      this.datetimePickerObj.monthsArray.secondMonth.days = this.makeDaysOfMonth(firstDayOfNextMonth);
      this.datetimePickerObj.monthsArray.secondMonth.month = [currentDateArray[0], currentDateArray[1] + 1];

      debugger;

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
    makeDaysOfMonth(startDate) { // startDate example : [1398,11,1]
      const isLeapYear = new PersianDate(startDate).isLeapYear(); // check if year is leap year
      let daysOfMonth = this.getDaysOfMonth(startDate[1], isLeapYear); // get count of days in the month
      let monthArray = [];

      for (let i = 1; i <= daysOfMonth; i++) {
        let dateObject = {};
        dateObject.isHoliday = false;
        dateObject.date = [startDate[0], startDate[1], i];
        dateObject.dayOfWeek = this.fixPersianNumber(new PersianDate(dateObject.date).format('d'));
        monthArray.push(dateObject);
      }
      return monthArray;
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
