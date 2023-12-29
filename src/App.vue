<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  

  <RouterView />
</template>

<style >
*{
  margin:0;
  padding: 0;
  list-style-type: none;
  text-decoration: none;
  box-sizing: border-box;
  font-size: 16px !important;
}
@media(max-width:768px){
  *{font-size: 14px !important;}
}
</style>
