<template>
  <div id="app">
    <main>
      <div class="container">
        <div class="search-box">
          <h1 class="text-center">Weather Reporter</h1>
          <br>
          <b-form-input placeholder="Enter City or Countery name" @keypress="fetchWeather" v-model="query">
          </b-form-input>
        </div>

        <div v-if="typeof res.main != 'undefined'">
          <div class="main-res mt-4">
            <p>{{ res.name }} - {{ res.sys.country }}</p>
            <p> {{ Math.floor(res.main.temp - 273.15) }} ℃ </p>
            <img :src="'http://openweathermap.org/img/w/' + res.weather[0].icon + '.png'" alt="">
          </div>

          <div class="extra-res">
            <b-row class="mt-4">
              <b-col>
                <div class="box">
                  <p>Date : {{ today }} - {{ time }}</p>
                </div>
              </b-col>
              <b-col>
                <div class="box">
                  <p>Humidity : {{ res.main.humidity }}</p>
                </div>
              </b-col>
              <b-col>
                <div class="box">
                  <p>Pressure : {{ res.main.pressure }}</p>
                </div>
              </b-col>
            </b-row>
            <b-row class="mt-3">
              <b-col>
                <div class="box">
                  <p>Feels Like : {{ res.main.feels_like }}</p>
                </div>
              </b-col>
              <b-col>
                <div class="box">
                  <p>Min Temp : {{ Math.floor(res.main.temp_min - 273.15) }} ℃</p>
                </div>
              </b-col>
              <b-col>
                <div class="box">
                  <p>Max Temp : {{ Math.floor(res.main.temp_max - 273.15) }} ℃</p>
                </div>
              </b-col>
            </b-row>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { BFormInput, BRow, BCol } from "bootstrap-vue";
export default {
  name: "App",
  components: {
    BFormInput, BRow, BCol
  },
  data() {
    return {
      API_key: "22b4a19ba4a7a1d4dceb35a0f0fd9c06",
      url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      res: {},
      date: new Date(),
      today: "",
      time: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url}weather?q=${this.query}&appid=${this.API_key}`)
          .then((res) => {
            return res.json();
          })
          .then((response) => {
            this.res = response;
            console.log(this.res);
          });
      }
    },
  },
  mounted() {
    this.today =
      this.date.getFullYear() +
      "-" +
      (this.date.getMonth() + 1) +
      "-" +
      this.date.getDate();
    this.time =
      this.date.getHours() +
      ":" +
      this.date.getMinutes() +
      ":" +
      this.date.getSeconds();
  },
};
</script>

<style>
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

#app {
  background-image: url("./assets/sky.jpg");
  background-size: cover;
}

.container {
  position: sticky;
  top: 5%;
}

main {
  min-height: 100vh;
}

.search-box {
  border-radius: 10px;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.6);
  border: 1px solid #f1f1f1;
  z-index: 2;
  padding: 20px;
  color: white;
}

.main-res {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background: white;
  font-size: 20px;
  font-weight: bold;
  border-radius: 10px 10px 0px 0px;
  padding: 10px 20px;
  align-items: center;
  border-bottom: solid 6px #f90000;
  box-shadow: 0px 4px 8px 1px #000000a6;
}

.main-res p,
.extra-res p {
  margin: 0px;
}

.row {
  align-items: flex-end;
}

.box {
  background: white;
  padding: 20px;
  border-radius: 10px 10px 0px 0px;
  border-bottom: solid 5px red;
  box-shadow: 0px 4px 8px 1px #000000a6;
  font-style: italic;
  font-weight: 600;
  font-size: 18px;
}
</style>
