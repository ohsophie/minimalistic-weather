<script>
  import axios from "axios";

  export default {
    data() {
      return {
        city: "",
        errorMes: "",
        info: null,
        iconLink: null,
        date: null,
        year: new Date().getFullYear(),
        defaultCities: [
          "Tokyo",
          "Hamburg",
          "Bratislava",
          "Chongqing",
          "Nikolayev",
          "Prague",
          "Geneva",
          "Philadelphia",
          "Ontario",
          "Dubai",
        ],
      };
    },
    computed: {
      cityName() {
        return "«" + this.city + "»";
      },
      currentCityDate() {
        return this.info.city.name + ", " + this.date;
      },
      cityTemp() {
        return this.info.list[0].main.temp + "°C";
      },
      cityMaxMinTemp() {
        return (
          "Max / min temperature: " +
          this.info.list[0].main.temp_max +
          " / " +
          this.info.list[0].main.temp_min +
          "°C"
        );
      },
      cityHumidity() {
        return "Humidity: " + this.info.list[0].main.humidity + "%";
      },
      cityWindSpeed() {
        return "Wind speed: " + this.info.list[0].wind.speed + " m/s";
      },
      cityPressure() {
        return "Pressure: " + this.info.list[0].main.pressure + " hPa";
      },
      cityRain() {
        if (this.info.list[0].rain == undefined) {
          return "We assume that there will be no rain today.";
        } else {
          return "Can be raining today, better take your umbrella.";
        }
      },
    },
    methods: {
      async getWeather() {
        this.errorMes = "";
        this.info = null;

        if (this.city.trim().length < 2) {
          this.errorMes = "We need a word longer than 1 symbol";
          return false;
        }
        this.errorMes = "";

        try {
          const response = await axios.get(
            "https://api.openweathermap.org/data/2.5/forecast",
            {
              params: {
                q: this.city,
                units: "metric",
                appid: "036ab933d97228ea2aa28ec3b780db9a",
              },
            }
          );

          this.info = response.data;
          this.iconLink = `https://openweathermap.org/img/wn/${this.info.list[0].weather[0].icon}@2x.png`;
          this.date = this.getLocalDate(this.info.city.timezone);
        } catch (error) {
          if (error.response && error.response.status === 404) {
            this.errorMes = "There might be a typo, check yourself, please.";
          } else if (!this.info.list || this.info.list.length === 0) {
            this.errorMes = "Weather data is not available for this location.";
            return;
          } else {
            this.errorMes = "There is a problem with server, try again later.";
          }
        }
      },
      getLocalDate(offset) {
        const nowUTC = new Date(
          Date.now() + new Date().getTimezoneOffset() * 60000
        );
        const localTime = new Date(nowUTC.getTime() + offset * 1000);

        const day = String(localTime.getDate()).padStart(2, "0");
        const month = String(localTime.getMonth() + 1).padStart(2, "0");
        const year = localTime.getFullYear();

        return `${day}.${month}.${year}`;
      },
    },
  };
</script>

<template>
  <div class="app">
    <div class="header">
      <h1>Minimalistic weather app</h1>
    </div>
    <div class="wrap">
      <p>
        You came here for some weather update? Let's check the weather in
        {{ city == "" ? "your city" : cityName }}.
      </p>
      <input
        type="text"
        v-model="city"
        :placeholder="
          'input city: for example, ' +
          defaultCities[Math.floor(Math.random() * defaultCities.length)] +
          '...'
        "
      />
      <button v-show="city != ''" @click="getWeather()">Get weather</button>
      <p class="error">{{ errorMes }}</p>
      <div class="response" v-if="info != null">
        <div class="info">
          <p>{{ cityMaxMinTemp }}</p>
          <p>{{ cityHumidity }}</p>
          <p>{{ cityWindSpeed }}</p>
          <p>{{ cityPressure }}</p>
          <p>{{ cityRain }}</p>
        </div>
        <div class="image">
          <img :src="iconLink" alt="weather icon" width="130" />
          <p class="current">{{ cityTemp }}</p>
          <p class="city">{{ currentCityDate }}</p>
        </div>
      </div>
    </div>
    <div class="footer">
      <p>© {{ year }} ohsophie. All rights reserved.</p>
    </div>
  </div>
</template>

<style scoped>
  .header {
    color: #355240;
    display: flex;
    justify-content: center;
    padding: 20px;
    padding-top: 0;
  }
  .response {
    color: #355240;
    display: flex;
    flex-direction: row;
  }
  .image {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 18pt;
    padding-left: 25px;
  }
  .info {
    font-size: 16pt !important;
  }
  .current {
    font-size: 26pt;
  }
  img {
    display: flex;
    align-self: center;
  }
  .error {
    color: #eb3d3d;
  }
  .wrap {
    width: 50vw;
    border-radius: 25px;
    padding: 20px;
    background: rgba(195, 222, 205, 0.4);
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #1d6b3f;
  }
  .wrap p {
    margin: 10px;
  }
  .wrap input {
    margin-top: 20px;
    margin-bottom: 20px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #b6f2cd;
    width: 300px;
    color: #fff;
    font-size: 12px;
    padding: 5px 8px;
    outline: none;
  }
  .wrap input:focus {
    border-bottom-color: #355240;
  }
  .wrap button {
    background-color: #355240;
    color: white;
    border: none;
    border-radius: 2px;
    cursor: pointer;
    padding: 10px 15px;
    margin-left: 10px;
    transition: transform 500ms ease;
  }
  .wrap button:hover {
    transform: scale(1.1);
  }
  .wrap button:active {
    background-color: #6fc192;
  }
  .footer {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    color: white;
    opacity: 0.7;
    text-align: center;
    font-size: 10pt;
  }
  @media screen and (max-width: 650px) {
    .wrap {
      background-color: transparent;
      width: 100vw;
      text-align: center;
    }
    .response {
      flex-direction: column !important;
    }
    .image {
      padding-left: 0px;
    }
    .header {
      padding-top: 30px;
    }
    .footer {
      position: static;
    }
  }
  @media screen and (max-width: 300px) {
    .wrap input {
      width: 100vw;
    }
  }
</style>
