<template>
  <div class="flex flex-col items-center mt-4 font-serif">
    <h3>Select delivery date : </h3>
    <DatePicker @selected-date="handleFilter($event)" />
    <MyProducts :filteredData="filteredData" />
  </div>
</template>

<script>
import api from "./assets/api.json";
import MyProducts from "./components/MyProducts.vue";
import DatePicker from "./components/DatePicker.vue";

export default {
  components: {
    DatePicker,
    MyProducts,
  },
  data() {
    return {
      filteredData: api.data,
    };
  },
  methods: {
    handleFilter(date) {
      // checking diff between selected date and current date in days
      const time = Math.floor(
        (date.getTime() - new Date().getTime()) / (24 * 3600 * 1000)
      );

      this.filteredData = api.data.filter((item) => {
        if (time + 1 >= item.times.makeDays) { // 1st condition check 
          if (this.checkAvailableDays(item, date)) { // 2nd condition check
            if (this.checkExcludedDays(item, date)) { // 3rd condition check
              return item; // if item passes all 3 conditions it will enter the array
            }
          }
        }
      });
    },
    checkAvailableDays(item, date) {
       //returns true if the item can be provided on the selected day of the week
      return item.times.availableDaysOfWeek.some(
        (dayOfWeek) => dayOfWeek === date.getDay() + 1 
      );
    },
    checkExcludedDays(item, date) {
      // returns true if the selected date is not on the excluded dates of the item
      if (item.times.excludeDates.length) { //check if there are any excluded dates
        return !item.times.excludeDates.some((exDate) => { // returns false if the selected date is an excluded date 
          const tempTime = new Date(exDate.date);
          const month = tempTime.getMonth() + 1;
          const year = tempTime.getFullYear();
          const dayOfWeek = tempTime.getDate();

          if (
            year === date.getFullYear() &&
            month === date.getMonth() + 1 &&
            dayOfWeek === date.getDate()
          ) {
            return true;
          }
        });
      }
      return true;
    },
  },
};
</script>