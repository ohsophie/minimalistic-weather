<script>
  export default {
    data() {
      return {
        city: "",
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
    },
    methods: {
      sendData() {
        this.$emit("city", { city: this.city });
        //   alert(this.city);
      },
    },
  };
</script>

<template>
  <div class="inputs">
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
    <button v-show="city != ''" @click="sendData()">Get weather</button>
  </div>
</template>

<style>
  .inputs {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  p {
    margin: 10px;
    color: #1d6b3f;
  }
  input {
    margin-top: 20px;
    margin-bottom: 20px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #b6f2cd;
    width: 300px;
    color: #fff;
    font-size: 16px;
    padding: 5px 8px;
    outline: none;
  }
  input:focus {
    border-bottom-color: #355240;
  }
  button {
    background-color: #355240;
    color: white;
    border: none;
    border-radius: 2px;
    cursor: pointer;
    padding: 10px 15px;
    margin-left: 10px;
    transition: transform 500ms ease;
  }
  button:hover {
    transform: scale(1.1);
  }
  button:active {
    background-color: #6fc192;
  }
  @media screen and (max-width: 300px) {
    input {
      width: 100vw;
    }
  }
</style>
