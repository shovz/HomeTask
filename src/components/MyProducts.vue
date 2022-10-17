<template>
  <div
    class="
      w-4/6
      grid
      sm:grid-cols-1
      md:grid-cols-2
      lg:grid-cols-3
      xl:grid-cols-4
      gap-4
      text-center
      mb-3
    "
  >
    <div
      v-for="item in filteredData"
      :key="item.id"
      class="shadow shadow-slate-400 p-3 rounded-md"
    >
      <div class="mr-3 flex flex-col items-end h-1/4">
        <h4 class="mb-2 font-rubik self-center">{{ item.name }}</h4>
        <p>מחיר : ₪ {{ item.price }}</p>
        <h4>
          משך הכנה :
          <span v-if="!item.times.makeDays">מיידי</span>
          <span v-if="item.times.makeDays === 1">יום אחד</span>
          <span v-if="item.times.makeDays > 1"
            >{{ item.times.makeDays }} ימים</span
          >
        </h4>
        <div class="flex flex-wrap flex-row-reverse">
          <h4>: זמין בימים</h4>
          <h4
            class="mr-1"
            v-for="(day, index) in item.times.availableDaysOfWeek"
            :key="day"
          >
            {{ weekDays[day - 1] }}
            <span v-if="item.times.availableDaysOfWeek.length > 1 && index != 0"
              >/</span
            >
          </h4>
        </div>
        <div
          class="flex flex-wrap flex-row-reverse"
          v-if="item.times.excludeDates.length"
        >
          <h4 class="ml-1">: למעט התאריכים*</h4>
          <h4
            ca
            v-for="(exDate, index) in item.times.excludeDates"
            :key="exDate.id"
          >
            {{ excludedDays(exDate.date) }}
            <span v-if="item.times.availableDaysOfWeek.length > 1 && index != 0"
              >,</span
            >
          </h4>
        </div>
      </div>
      <div class="p-2 h-3/4 flex items-end justify-center">
        <img class="lg:h-64" :src="item.imgUrl" :alt="item.name" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["filteredData"],
  data() {
    return {
      weekDays: ["'א", "'ב", "'ג", "'ד", "'ה", "'ו", "'ש"],
    };
  },
  methods: {
    excludedDays(exDate) {
      const date = new Date(exDate);
      const year = date.getFullYear().toString().substring(2);
      return date.getDate() + "/" + (date.getMonth() + 1) + "/" + year;
    },
  },
};
</script>