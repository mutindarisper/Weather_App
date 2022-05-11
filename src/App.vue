<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" name="" id="" class="search-bar"
         placeholder="Search..."
         v-model="query"
         @keypress="fetchWeather">
      </div>

     <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}},  {{weather.sys.country}}</div>
          <div class="date">{{ pickCurrentDate()}}</div>
        </div>


        <div class="weather-box">
         <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
         <div class="weather">{{ weather.weather[0].main }}</div>
         <div icon="sunny" class="sun-icon" v-if="sunny"> </div>
          <div icon="cloudy" class="cloud-icon" v-if="cloudy"> </div>
           <div icon="rainy" class="rain-icon" v-if="rainy"> </div>
       
          
        </div>

      </div>
    </main>
    
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return{
      api_key: '60938220bb9fc41c7b5444ae8ebbfc09',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      sunny: false,
      cloudy: false,
      rainy: false,
    }
  },
  components: {
  
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then( response => {
          return response.json();
        })
        .then( this.setResults)
      }

    },
    setResults(results) {
      
      this.weather = results
      if(this.weather.weather[0].main.includes('Clouds')) {
        this.cloudy = true;
        this.sunny = false;
        this.rainy= false;

      }
      if(this.weather.weather[0].main.includes('Rain')) {
        this.cloudy = false;
        this.sunny = false;
        this.rainy= true;

      }
      if(this.weather.weather[0].main.includes('Clear')) {
        this.cloudy = false;
        this.sunny = true;
        this.rainy= false;

      }
     
    },
    pickCurrentDate() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }


  }
}
</script>

<style scoped>
@import "./assets/app.css";

</style>
