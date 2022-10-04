<template>
  <VueTable :rows="table" :titles="titles" />
</template>

<script>
import VueTable from "./components/VueTable.vue";

export default {
  name: "App",
  components: {
    VueTable,
  },
  data() {
    return {
      table: [
        { id: 1, date: "12:10:22", name: "Coleo", quantity: 1, distance: 20 },
        { id: 2, date: "12:10:22", name: "Aol", quantity: 444, distance: 50 },
        {
          id: 3,
          date: "12:10:22",
          name: "BAd",
          quantity: 100,
          distance: 2,
        },
        {
          id: 4,
          date: "12:10:45",
          name: "Head",
          quantity: 1000,
          distance: 266,
        },
        { id: 5, date: "15:10:22", name: "Coo", quantity: 435, distance: 900 },
        { id: 6, date: "19:10:22", name: "Boom", quantity: 334, distance: 128 },
        {
          id: 7,
          date: "02:10:22",
          name: "Text",
          quantity: 480,
          distance: 90,
        },
        {
          id: 8,
          date: "16:10:55",
          name: "Head",
          quantity: 10080,
          distance: 66,
        },
      ],
      titles: [
        { id: 1, lable: "Name" },
        { id: 2, lable: "Quantity" },
        { id: 3, lable: "Distance" },
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
