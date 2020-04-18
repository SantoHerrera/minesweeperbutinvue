<template>
  <div id="app">
    <h2>stopWatch</h2>
    <p>{{hours}}:{{minutes}}:{{seconds}}</p>
    <button v-on:click="clickTHis">manualllyCount</button>
    <button v-on:click="iSaidStaaaap">stop</button>
    <button v-on:click="iSaidStart">start</button>
  </div>
</template>

<script>
export default {
  name: "StopWatch",
  data: function() {
    return {
      hours: 0,
      minutes: 0,
      seconds: 0,
      isRunning: true
    };
  }
};
</script>>