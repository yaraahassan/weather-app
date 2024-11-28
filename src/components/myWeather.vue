<template>
  <!-- container -->
  <div class="container d-flex">
    <!-- left-sec -->
    <div class="left-sec p-3" style="width: 50%; height: 400px">
      <h2 class="mb-0">Today</h2>
      <p>{{ date }}</p>
      <p>{{ time }}</p>
      <h2 class="location">
        <i class="fa-solid fa-location-crosshairs"></i>{{ name }},<small>{{
          country
        }}</small>
      </h2>

      <div class="desc">
        <h1 class="temp">{{ temperature }}&deg;</h1>
        <p class="description">{{ description }} <img :src="iconUrl" /></p>
      </div>
    </div>
    <!-- end left-sec -->

    <!-- right-sec -->
    <div class="right-sec" style="width: 50%">
      <table class="m-4">
        <tr>
          <th>SEA LEVEL</th>
          <td>{{ seaLevel }}</td>
        </tr>
        <tr>
          <th>HUMIDITY</th>
          <td>{{ humidity }}</td>
        </tr>
        <tr>
          <th>WIND</th>
          <td>{{ wind }}</td>
        </tr>
      </table>

      <!-- call comonent -->
      <dayWeather :cityname="citynnname" />

      <div class="d-flex justify-content-center">
        <button class="btnn btn-primary"  @click="changeLocation">Change Location</button>
      </div>
    </div>
    <!-- end right sec -->
  </div>
  <!--end main-container -->
</template>

<script>
import axios from "axios";
import dayWeather from "@/components/dayWeather.vue";

export default {
  name: "myWeather",
  props: {
    city: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      citynnname: this.city,
      name: null,
      temperature: null,
      description: null,
      time: null,
      date: null,
      iconUrl: null,
      country: null,
      seaLevel: null,
      humidity: null,
      wind: null,
    };
  },
  components: {
    dayWeather,
  },
  methods: {
    async getInfo() {
      console.log(this.city);

      const response = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=44d2444f5e3a0dcfbc93f071e3eb2e29`
      );
      console.log(response);
      const weatherData = response.data;
      console.log(weatherData);
      this.name = weatherData.name;
      this.temperature = Math.round(weatherData.main.temp);
      this.description = weatherData.weather[0].description;
      this.country = weatherData.sys.country;
      this.seaLevel = weatherData.main.sea_level;
      this.humidity = weatherData.main.humidity;
      this.wind = weatherData.wind.speed;

      //get time
      const obj = new Date();
      const hours = obj.getHours();
      const min = obj.getMinutes();
      const sec = obj.getSeconds();
      this.time = `${hours}:${min}:${sec}`;

      //date
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const day = days[obj.getDay()];
      const month = months[obj.getMonth()];
      const year = obj.getFullYear();
      this.date = `${day}-${month}-${year}`;

      //icon symbol
      const icon = weatherData.weather[0].icon;

      //get icon picture using url

      this.iconUrl = `https://openweathermap.org/img/wn/${icon}.png`;
    },
    changeLocation(){
      window.location.reload();
    }
  },
  mounted() {
    this.getInfo(); // استدعاء getInfo
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.left-sec {
  background-image: url("https://plus.unsplash.com/premium_photo-1673603988651-99f79e4ae7d3?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8d2VhdGhlciUyMGFwcHxlbnwwfHwwfHx8MA%3D%3D") !important;
  background-size: cover;
  color: #fff;
  position: relative;
  border-radius: 20px;
  background-color: rgba(0, 0, 0, 0.5);
}
p {
  font-weight: 300;
  margin: 0;
}
i {
  font-size: 23px;
  font-weight: 600;
}
.location {
  font-size: 28px;
  font-weight: 600;
}
.desc {
  position: absolute;
  bottom: 0;
}
.temp {
  font-size: 70px;
}
.description {
  font-size: 25px;
  font-weight: 600;
}
.left-sec:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
}
.right-sec {
  border-radius: 20px;
  background-color: #212730;
}
table {
  width: 100%;
}
tr {
  display: flex;
  justify-content: space-between;
}
th,
td {
  padding: 10px;
  margin-right: 90px;
  color: #fff;
}
th {
  font-size: 17px;
}
.btnn {
  background-image: linear-gradient(to right, cyan, magenta);
  padding: 5px;
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  border-radius: 5px;
}
</style>
