<template>
  <div class="app">
    <div class="header h-100 container p-5">
      <h1 class="mb-4">Weather App</h1>
      <div class="search d-flex justify-content-center h-100">
        <input
          type="text"
          v-model="city"
          class="input form-control w-50 mx-2"
          placeholder="Enter a city"
         
        />
        <button class="btn btn-primary" @click="searchWeather">
          search <i class="fa-solid fa-magnifying-glass"></i>
        </button>
      </div>
    </div>
  </div>
  <MyWeather :city="city" v-if="showWeather"/>
</template>

<script>
import MyWeather from "./components/myWeather.vue";

export default {
  name: "App",
  components: {
    MyWeather,
  },
  data() {
    return {
      city: "",
      showWeather: false,
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    },
    
  },
};
</script>

<style>
body {
  background-color: #000 !important;
}

.header {
  background-color: #212730;
  color: #fff;
  border-radius: 5px;
}
h1 {
  text-align: center;
}

i {
  font-size: 11px;
}
.btn {
  background-image: linear-gradient(to right, cyan, magenta);
}
</style>
