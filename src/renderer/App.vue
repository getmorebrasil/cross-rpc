<template>
  <div id="app">
    <header-app @triggerReq="triggerReqFunc"></header-app>
    <router-view :triggerReq="triggerReq"></router-view>
  </div>
</template>

<script>
import HeaderApp from "./components/header";
export default {
  name: "insomnia-rpc",
  components: { HeaderApp },
  data: () => ({
    triggerReq: false
  }),
  methods: {
    triggerReqFunc(req) {
      this.triggerReq = req;
    }
  }
};
</script>

<style>
/* CSS */
</style>
