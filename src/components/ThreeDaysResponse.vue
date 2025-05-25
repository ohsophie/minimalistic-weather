<script>
  export default {
    data() {},
    props: ["info", "city", "date"],
    computed: {
      cityTempToday() {
        return Math.floor(this.info.list[0].main.temp) + "°C";
      },
      cityTempDayOne() {
        return Math.floor(this.info.list[5].main.temp) + "°C";
      },
      cityTempDayTwo() {
        return Math.floor(this.info.list[11].main.temp) + "°C";
      },
      cityTempDayThree() {
        return Math.floor(this.info.list[17].main.temp) + "°C";
      },
    },
  };
</script>

<template>
  <div class="info">
    <div class="subheader">
      <p>Here is the temperature chart for next three days in {{ city }}.</p>
    </div>
    <div class="dateTemp">
      <div class="date">
        <p class="d">{{ date }}</p>
        <p class="t">
          Expected temperature: <b>{{ cityTempToday }}</b>
        </p>
      </div>
      <div class="date">
        <p class="d">{{ Number(date.slice(0, 2)) + 1 + date.slice(2) }}</p>
        <p class="t">
          Expected temperature: <b>{{ cityTempDayOne }}</b>
        </p>
      </div>
      <div class="date">
        <p class="d">{{ Number(date.slice(0, 2)) + 2 + date.slice(2) }}</p>
        <p class="t">
          Expected temperature: <b>{{ cityTempDayTwo }}</b>
        </p>
      </div>
      <div class="date">
        <p class="d">{{ Number(date.slice(0, 2)) + 3 + date.slice(2) }}</p>
        <p class="t">
          Expected temperature: <b>{{ cityTempDayThree }}</b>
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .info {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .subheader p {
    margin: 20px;
    margin-bottom: 0;
    font-size: 16pt;
    font-weight: 700;
  }
  .date {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 16pt;
  }
  b {
    font-size: 20pt;
  }
  .t {
    font-size: 16pt;
    margin-top: 0;
  }
  .dateTemp {
    margin: 20px;
  }
  .d {
    font-weight: 800;
    text-decoration: underline;
  }
</style>
