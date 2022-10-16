<template>
  <div class="flex  flex-col  items-center mt-4">
    <div class="flex  flex-col items-center justify-center mb-4">
      <Datepicker
        v-model="date"
        :minDate="new Date()"
        :maxDate="endDate"
        @update:modelValue="handleDate"

        showNowButton
        :enableTimePicker="false"
         :textInputOptions="textInputOptions"
      ></Datepicker>
      <div class="mx-10 w-fit" v-if="filteredData.length">
        <h4>
          Selected Date : {{ date.getDate() }} / {{ date.getMonth() + 1 }} /
          {{ date.getFullYear() }}
        </h4>
      </div>
    </div>
    <MyProducts :filteredData="filteredData" :date="date" />
  </div>
</template>

<script>
import api from "./api.json";
import MyProducts from "./components/MyProducts.vue";
import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";
import { ref } from 'vue';

export default {
  name: "App",
  components: {
    Datepicker,
    MyProducts,
  },
  data() {
    return {
      apiData: api.data,
      filteredData: [],
    };
  },
  methods: {
    handleDate() {
      const time = Math.floor(
        (this.date.getTime() - new Date().getTime()) / (24 * 3600 * 1000)
      );

      this.filteredData = this.apiData.filter((item) => {
        if (time + 1 >= item.times.makeDays) {
          if (this.checkavailableDays(item)) {
            if (this.checkExcludedDays(item)) {
              return item;
            }
          }
        }
      });
    },
    checkavailableDays(item) {
      return item.times.availableDaysOfWeek.some(
        (weekday) => weekday === this.date.getDay() + 1
      );
    },
    checkExcludedDays(item) {
      if (item.times.excludeDates.length) {
        return !item.times.excludeDates.some((exDate) => {
          const tempTime = new Date(exDate.date);
          const month = tempTime.getMonth() + 1;
          const year = tempTime.getFullYear();
          const weekDay = tempTime.getDate();

          if (
            year === this.date.getFullYear() &&
            month === this.date.getMonth() + 1 &&
            weekDay === this.date.getDate()
          ) {
            console.log("tempTime", weekDay, month, year);
            console.log(
              "this.date",
              this.date.getDate(),
              this.date.getMonth() + 1,
              this.date.getFullYear()
            );
            return true;
          }
        });
      }
      return true;
    },
  },
  setup() {
    const date = new Date();
    const endDate = new Date();
    endDate.setDate(endDate.getDate() + 14);
    // const format = () => {
    //   return `Select Date`;
    // };
    const textInputOptions = ref({
          format: 'MM.dd.yyyy'
        })
    return {
      date,
      endDate,
      // format,
      textInputOptions
    };
  },
};
</script>