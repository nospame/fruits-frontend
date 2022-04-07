<script>
import axios from 'axios'
export default {
  data: function () {
    return {
      message: "This is the Fruits App",
      fruits: [],
      currentFruit: {},
      newFruit: {},
      errors: []
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
    },
    showFruit: function (fruit) {
      console.log('in fruit show');
      this.currentFruit = fruit;
      console.log(this.currentFruit);
      document.querySelector("#fruit-details").showModal();
    },
    createFruit: function (fruit) {
      console.log('in fruit create');
      axios.post('/fruits', fruit)
        .then(response => {
          console.log(response.data);
          this.fruits.push(response.data);
          this.newFruit = {};
          this.errors = [];
        })
        .catch(errors => {
          this.errors = errors.response.data.errors;
          console.log(this.errors);
        })
    },
    updateFruit: function (fruit) {
      console.log('in fruit update');
      axios.patch(`/fruits/${fruit.id}`, fruit)
        .then(response => {
          console.log(response.data);
          this.errors = [];
        })
    },
    destroyFruit: function (fruit) {
      console.log('in fruit destroy');
      axios.delete(`/fruits/${fruit.id}`)
        .then(response => {
          console.log(response.data);
          this.fruits.splice(this.fruits.indexOf(fruit));
        })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      Name:
      <input v-model="newFruit.name" />
    </p>
    <p>
      Color:
      <input v-model="newFruit.color" />
    </p>
    <p>
      Family:
      <input v-model="newFruit.family" />
    </p>
    <p>
      Image URL:
      <input v-model="newFruit.image_url" />
    </p>

    <p v-for="error in errors" class="error">{{ error }}</p>

    <button v-on:click="createFruit(newFruit)">Add Fruit</button>
    <div v-for="fruit in fruits">
      <p>
        <img v-bind:src="fruit.image_url" class="index-img" />
        <br />
        {{ fruit.name }}
        <br />
        <button v-on:click="showFruit(fruit)">Details</button>
      </p>
    </div>
    <dialog id="fruit-details">
      <form method="dialog">
        <img v-bind:src="currentFruit.image_url" class="show-img" />
        <p>
          Name:
          <input v-model="currentFruit.name" />
        </p>
        <p>
          Family:
          <input v-model="currentFruit.family" />
        </p>
        <p>
          Color:
          <input v-model="currentFruit.color" />
        </p>
        <p>
          Image URL:
          <input v-model="currentFruit.image_url" />
        </p>
        <p>
          <button v-on:click="updateFruit(currentFruit)">Save Changes</button>

          <button v-on:click="destroyFruit(currentFruit)">Delete Fruit</button>
        </p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
.index-img {
  width: 200px;
}

.show-img {
  width: 450px;
}

.error {
  color: crimson;
  font-style: italic;
}
</style>