<template>
  <!-- <span >loading</span> -->
   <div v-if="loading">Loading</div>
   <div v-else-if="forecastt.length === 0">No data available</div>
  <ul v-else>
    <li v-for="day in forecastt.slice(0, 4)" :key="day.date">
      <div><img :src="day.iconUrl"/></div>
      <div>{{day.date}}</div>
      <div class="last">{{day.temp}}&deg;</div>
    </li>

    
  </ul>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "dayWeather",
  props: {
    cityname: {
      type: String,
      required: true,
    },
  },
   data(){
    return{
      forecastt:[],
      loading:true,
      iconUrl:null,
    };
   },
   methods: {
  async fetchWeatherData() {
    const apiKey = "44d2444f5e3a0dcfbc93f071e3eb2e29";
    const city = this.cityname;
    const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;

    try {
      // بدء تحميل البيانات
      this.loading = true;

      // طلب البيانات من API
      const response = await axios.get(apiUrl);
      const forecast = response.data.list;

      // معالجة البيانات
      const filterForecast = forecast.map((item) => {
        return {
          temp: item.main.temp,
          desc: item.weather[0].description,
          iconUrl: `https://openweathermap.org/img/wn/${item.weather[0].icon}.png`,
          date: moment(item.dt_txt.split(" ")[0]).format("YYYY-MM-DD"),
        };
      });

      // تعيين البيانات في `forecastt`
      this.forecastt = filterForecast;

    } catch (error) {
      console.error("Error fetching weather data:", error);
    } finally {
      // إنهاء حالة التحميل
      this.loading = false;
    }
  },
},

  mounted() {
    this.fetchWeatherData();
  },
};
</script>

<style>
ul {
  padding-top: 20px;
  box-shadow: 0px 4px 8px 0 rgba(0, 0, 0, 0.2),
    0px 6px 20px 0 rgba(0, 0, 0, 0.19);
  /* margin: 25px;*/
  border-radius: 20px;
  padding:10px;
}
li {
  border-radius: 20px;
  display: inline-block;
  color: white;
  padding: 10px;
  background-color: rgb(89, 82, 123);
  text-align: center;
  margin-right: 30px;
  margin-left: 18px;
}
li:hover {
  transform: scale(1.1);
}
div {
  margin-bottom: 20px;
}
.last {
  margin-bottom: 0;
}
</style>
