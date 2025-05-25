<script>
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
    margin-top: 50px;
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
      padding: 25px;
      text-align: center;
      font-size: 20px;
      margin-top: 0px;
    }
  }
</style>
