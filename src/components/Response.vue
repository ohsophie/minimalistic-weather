<script>
  export default {
    props: ["info", "iconLink", "date"],
    computed: {
      currentCityDate() {
        return this.info.city.name + ", " + this.date;
      },
      cityTemp() {
        return Math.floor(this.info.list[0].main.temp) + "°C";
      },
      cityMaxMinTemp() {
        return (
          "Max / min temperature: " +
          Math.floor(this.info.list[0].main.temp_max) +
          " / " +
          Math.floor(this.info.list[0].main.temp_min) +
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
      cityClouds() {
        if (this.info.list[1].clouds.all == 0) {
          return "Sky is clear, no clouds.";
        } else if (
          this.info.list[1].clouds.all > 0 &&
          this.info.list[1].clouds.all <= 30
        ) {
          return "A little bit cloudy";
        } else {
          return "It is cloudy.";
        }
      },
    },
  };
</script>

<template>
  <div class="response" v-if="info != null">
    <div class="info">
      <p>{{ cityMaxMinTemp }}</p>
      <p>{{ cityHumidity }}</p>
      <p>{{ cityWindSpeed }}</p>
      <p>{{ cityPressure }}</p>
      <p>{{ cityClouds }}</p>
      <p>{{ cityRain }}</p>
    </div>
    <div class="image">
      <img :src="iconLink" alt="weather icon" width="130" />
      <p class="current">{{ cityTemp }}</p>
      <p class="city">{{ currentCityDate }}</p>
    </div>
  </div>
</template>

<style scoped>
  .response {
    color: #355240;
    display: flex;
    flex-direction: row;
  }
  .image {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 20pt;
    padding-left: 25px;
  }
  .info {
    margin-top: 15px;
    font-size: 18pt !important;
  }
  .current {
    font-size: 30pt;
    font-weight: 800;
  }
  img {
    display: flex;
    align-self: center;
  }

  @media screen and (max-width: 650px) {
    .response {
      flex-direction: column !important;
    }
  }
</style>
