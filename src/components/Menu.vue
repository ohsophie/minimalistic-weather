<script>
  export default {
    data() {
      return {
        active: 0,
      };
    },
    methods: {
      styleMenuButton(x) {
        const y = x == 1 ? 0 : 1;
        const button = document.getElementById("b" + x);
        button.style.backgroundColor = "#6fc192";
        const buttonPrev = document.getElementById("b" + y);
        buttonPrev.style.backgroundColor = "transparent";
        this.sendStatus();
      },
      sendStatus() {
        this.$emit("status", { active: this.active });
      },
    },
  };
</script>

<template>
  <div class="menu">
    <button
      class="menuBut"
      id="b0"
      @click="(active = 0), styleMenuButton(active)"
    >
      Today
    </button>
    <button
      class="menuBut"
      id="b1"
      @click="(active = 1), styleMenuButton(active)"
    >
      3 days forecast
    </button>
  </div>
</template>

<style scoped>
  #b0 {
    background-color: #6fc192;
  }
  .menu {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    padding-top: 0;
    margin-top: 0;
  }

  .menuBut {
    width: 50%;
    background-color: transparent;
    color: #1d6b3f;
    font-size: 13pt;
    border-radius: 15px;
  }

  .menuBut:hover {
    font-size: 14pt;
    font-weight: 500;
    transition: transform 2000ms ease;
  }
  .menuBut:active {
    font-size: 14pt;
    background-color: transparent;
  }

  @media screen and (max-width: 650px) {
    .menu {
      flex-direction: column;
      align-items: center;
    }
    .menuBut {
      width: 90%;
    }
  }
</style>
