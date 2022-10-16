<template>
  <div>
      <Datepicker
    v-model="date"
    :minDate="new Date()"
    :maxDate="endDate"
     @update:modelValue="handleDate" 
  ></Datepicker>
    <button @click="handleClick">Learn More</button>
  </div>
</template>

<script>
import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";
import { ref } from "vue";

export default {
  components: { Datepicker },
  emits:['selected-date','topic'],
  setup() {
    const date = ref(new Date());
    const endDate = new Date();
    endDate.setDate(endDate.getDate() + 14);

    return {
      date,
      endDate,
    };
  },
  methods:{
    handleDate ()  {
      const day = this.date.getDay();
      const month = this.date.getMonth();
      const year = this.date.getFullYear();
      const weekDay = this.date.getDate();
      console.log(day,month,year,weekDay);
      this.$emit('selected-date',this.date);
      
    },
    handleClick(){
      console.log("aa");
      this.$emit('topic',12)
    }
  }
};
</script>
