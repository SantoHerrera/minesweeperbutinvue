<template>
  <div id="app">
    <StopWatch />
    <HelloWorld />
    {{idk}}
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import StopWatch from "./components/StopWatch.vue";


export default {
  name: "App",
  idk: "linked",
  data: function() {
    return {
      idk: "fuck"
    };
  },
  components: {
    HelloWorld,
    StopWatch
  }
};
</script>

