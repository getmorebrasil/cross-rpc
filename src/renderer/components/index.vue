<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.32.0/codemirror.min.js"></script>
<template>
  <article-app id="wrapper" :triggerReq="triggerReq"></article-app>
</template>

<script>
import ArticleApp from "./article";
import { setTimeout } from "timers";
export default {
  name: "index",
  components: { ArticleApp },
  props: {
    triggerReq: {
      type: Boolean | Object,
      default: false
    }
  },
  methods: {
    open(link) {
      this.$electron.shell.openExternal(link);
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
}

#wrapper {
  background-color: #3c3c3c;
  color: #e0e0e0;
  height: 90vh;
  padding: 10px;
  width: 100%;
  position: relative;
}
</style>
