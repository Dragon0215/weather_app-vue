<template>
  <div class="app" :class="typeof weather.main != 'undefined' ? wmain : ''">
    <div class="container row">
      <div class="col-6 srch">
        <input @keypress="search" v-model="query" type="text" class="form-control txt" placeholder="Search...">
      </div>
      <div class="col-6 info" v-if="typeof weather.main != 'undefined'">
        <h1 class="region">{{ weather.name }} {{ weather.sys.country }}</h1>
        <p class="date">{{ dateBuilder() }}</p>
        <h1 class="temp">{{ Math.round(weather.main.temp) }}<span>°C</span></h1>
        <h3 class="weather">{{ wmain }}</h3>
        <h2 class="min-max">{{ Math.round(weather.main.temp_max) }}/{{ Math.round(weather.main.temp_min) }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      key: "a129b2060568058717d96e675f91a2d1",
      baseurl: "https://api.openweathermap.org/data/2.5/",
      query: '',
      wmain: '',
      weather: {}
    }
  },
  methods: {
    search(e) {
      if (e.key == 'Enter') {
        fetch(`${this.baseurl}weather?q=${this.query}&units=metric&APPID=${this.key}`)
          .then((weather) => {
            return weather.json();
          })
          .then(this.displayResults);
      }
    },
    displayResults(res) {
      this.weather = res;
      this.query = '';
      this.wmain = this.weather.weather[0].main;
    },
    dateBuilder() {
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
<style scoped></style>
