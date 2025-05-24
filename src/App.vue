<script>
  import axios from "axios";
  import Wrap from "./components/Wrap.vue";

  export default {
    components: { Wrap },
    data() {
      return {
        year: new Date().getFullYear(),
      };
    },
  };
</script>

<template>
  <div class="app">
    <div class="header">
      <h1>Minimalistic weather app</h1>
    </div>
    <Wrap />
    <div class="footer">
      <p>© {{ year }} ohsophie. All rights reserved.</p>
    </div>
  </div>
</template>

<style scoped>
  .app {
    min-height: 100%;
    display: flex;
    flex-direction: column;
  }
  .header {
    color: #355240;
    display: flex;
    justify-content: center;
    padding: 20px;
    padding-top: 0;
  }
  .footer {
    margin-top: 40px;
    opacity: 0.4;
    text-align: center;
    font-size: 10pt;
  }
  .footer p {
    color: white;
  }
  @media screen and (max-width: 650px) {
    .header {
      padding-top: 20px;
      text-align: center;
      font-size: 20px;
    }
  }
</style>
