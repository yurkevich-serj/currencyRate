<template>
  <p>Официальный курс {{rate.Cur_Scale}} {{rate.Cur_Abbreviation}} на {{rate.Date}} равен
    {{rate.Cur_OfficialRate}} бел.руб</p>
</template>

<script>
  export default {
    name: "CurrencyTable",
    props: ['rate']
  }
</script>

<style scoped>
 p {
   font-size: 20px;
 }
</style>
