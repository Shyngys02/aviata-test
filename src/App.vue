<template>
  <div id="app">
    <air-tickets/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import AirTickets from './components/AirTickets.vue';

@Component({
  components: {
    AirTickets,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">

</style>
