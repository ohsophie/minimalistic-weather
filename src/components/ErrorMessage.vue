<script>
  export default {
    props: ["error"],
  };
</script>

<template>
  <p>Error: {{ error }}</p>
</template>

<style scoped>
  p {
    color: white;
    background-color: rgba(232, 110, 110, 0.5);
    border-radius: 5px;
    padding: 10px;
    padding-left: 30px;
    padding-right: 30px;
  }
  @media screen and (max-width: 300px) {
    p {
      width: 70vw;
    }
  }
</style>
