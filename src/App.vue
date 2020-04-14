<template>
  <div id="app">
    <HelloWorld />
    {{idk}}
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  idk: "linked",
  data: function() {
    return {
      idk: "fuck"
    };
  },
  components: {
    HelloWorld
  }
};
</script>

