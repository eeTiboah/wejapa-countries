<template>
    <div>
      <app-countries></app-countries>
    </div>
</template>

<script>
import Countries from './components/Countries.vue';
export default {
  components: {
    'app-countries': Countries
  }
}
</script>

<style>
body{
  margin: 0px;
}
* {
  box-sizing: border-box;
  margin: 0px;
}

body{
    font-size: 1.5em;
}
</style>
