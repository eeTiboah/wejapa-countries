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
  /* background-image: linear-gradient(to bottom right, #d3f8e2, #a9def9); */
  font-family: Georgia, 'Times New Roman', Times, serif;
  font-weight: 550;
  font-size: 1.3em;

}
* {
  box-sizing: border-box;
  margin: 0px;
}

</style>
