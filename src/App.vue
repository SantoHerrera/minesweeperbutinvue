<template>
  <div id="app">
    <HelloWorld/>
    {{idk}}
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
//import StopWatch from "./components/StopWatch.vue";


export default {
  name: "App",
  idk: "linked",
  data: function() {
    return {
      idk: ""
    };
  },
  components: {
    HelloWorld
    //StopWatch
  }
};
</script>

