<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "This is the Fruits App",
      fruits: []
    };
  },
  created: function () {
    this.indexFruits();
  },
  methods: {
    indexFruits: function () {
      console.log('in fruits index');
      axios.get('/fruits')
        .then(response => {
          console.log(response.data);
          this.fruits = response.data;
        })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
  <div v-for="fruit in fruits">{{ fruit.name }}</div>
</template>

<style></style>