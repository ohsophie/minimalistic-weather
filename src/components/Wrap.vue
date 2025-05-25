<script>
  import axios from "axios";

  import Input from "./Input.vue";
  import Error from "./ErrorMessage.vue";
  import Response from "./Response.vue";
  import ThreeDays from "./ThreeDaysResponse.vue";
  import Menu from "./Menu.vue";
  import ChartComponent from "./Chart.vue";

  export default {
    components: { Input, Error, Response, ThreeDays, Menu, ChartComponent },
    data() {
      return {
        forecastData: [],
        task: 0,
        city: "",
        errorMes: "",
        info: null,
        iconLink: null,
        date: null,
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

    methods: {
      readCity(readData) {
        this.city = readData.city;
        this.getWeather();
      },
      readMenuStatus(status) {
        this.task = status.active;
      },
      async getWeather() {
        this.errorMes = "";
        this.info = null;

        if (this.city.trim().length < 2) {
          this.errorMes = "A city name must be longer than 1 symbol.";
          return false;
        }

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
          this.forecastData = this.info.list.slice(0, 24);
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
  <div class="wrap">
    <Menu @status="readMenuStatus" />
    <hr />
    <Input @city="readCity" />
    <Error v-show="errorMes != ''" :error="this.errorMes" />
    <Response
      v-if="info != null && task == 0"
      :info="this.info"
      :iconLink="this.iconLink"
      :date="this.date"
    />
    <ThreeDays
      v-if="info != null && task == 1"
      :city="this.city"
      :date="this.date"
      :info="this.info"
    />

    <ChartComponent
      v-if="forecastData.length"
      v-show="task == 1"
      :forecastData="forecastData"
    />
  </div>
</template>

<style scoped>
  .wrap {
    border-radius: 25px;
    padding: 30px;
    padding-top: 5px;
    margin: 10px;
    background: rgba(195, 222, 205, 0.3);
    color: #1d6b3f;
    align-self: center;
    width: 50dvw;
  }
  hr {
    border-top: 0.5px solid #1d6b3f;
    border-bottom: 0px;
  }

  @media screen and (max-width: 650px) {
    .wrap {
      width: 70vw;
      text-align: center;
    }
  }
</style>
