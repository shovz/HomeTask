<template>
  <div>
    <Datepicker
      class="flex flex-col items-center justify-center mb-4"
      v-model="date"
      :minDate="new Date()"
      :maxDate="endDate"
      @update:modelValue="handleDate()"
      showNowButton
      :enableTimePicker="false"
    ></Datepicker>
  </div>
</template>
 
<script>
import Datepicker from "@vuepic/vue-datepicker";
import { ref } from "vue";

export default {
  components: { Datepicker },
  emits: ["selected-date"],
  methods: {
    handleDate() {
      this.$emit("selected-date", this.date);
    },
  },

  setup() {
    const date = ref(new Date());
    const endDate = new Date();
    endDate.setDate(endDate.getDate() + 14);
    return {
      date,
      endDate,
    };
  },
};
</script>
