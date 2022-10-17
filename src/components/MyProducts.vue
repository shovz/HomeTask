<template>
  <div class="w-1/2 grid sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 text-center mb-3 ">
    <div
      v-for="item in filteredData"
      :key="item.id"
      class="shadow 
      shadow-slate-400 
      p-3 
      rounded-md 
      flex 
      flex-col
      items-center"
    >
      <h4 class="mb-3 font-rubik">{{ item.name }}</h4>
      <h4>price : {{ item.price }}</h4>
      <h4>makeDays : {{ item.times.makeDays }}</h4>
      <img class="p-2 md:w-32 lg:w-36 xl:w-48" :src="item.imgUrl" :alt="item.name" />
    </div>
  </div>
</template>

<script>
export default {
  props: ["filteredData"],
};
</script>