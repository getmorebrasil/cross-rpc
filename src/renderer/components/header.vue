<template>
  <header id="header">
    <button type="button" class="btn wide">
      <h1 class="no-pad text-left">Insomnia-RPC</h1>
    </button>
    <div class="path">
      <input type="button" class="ok" value="OK" v-on:click="triggerReq" />
      <input
        class="queue"
        type="text"
        value="fila_de_teste"
        placeholder="queue"
        v-on:input="getQueue($event)"
      />
      <input
        class="host"
        type="text"
        value="localhost:5672"
        placeholder="host"
        v-on:input="getHost($event)"
      />
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  data: () => ({
    host: "",
    queue: ""
  }),
  methods: {
    open(link) {
      this.$electron.shell.openExternal(link);
    },
    triggerReq() {
      this.$emit("triggerReq", { host: this.host, queue: this.queue });
      const debouce = setTimeout(() => {
        this.$emit("triggerReq", false);
      }, 1000);
    },
    getQueue(event) {
      this.queue = event.srcElement.value;
    },
    getHost(event) {
      this.host = event.srcElement.value;
    }
  },
  mounted() {
    this.host = this.$el.querySelector("input.host").value;
    this.queue = this.$el.querySelector("input.queue").value;
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

#header {
  background: linear-gradient(165deg, #6e60cc, #5c5cb8);
  width: 100%;
  height: 50px;
}

.btn {
  text-align: center;
  background: var(--color-bg);
  border: 1px solid transparent;
  color: #fff;
  margin-top: 12px;
  margin-left: 10px;
}
h1 {
  font-size: 18px;
}
.path {
  width: 70%;
  height: 50px;
  float: right;
  padding: 5px;
  background-color: #fff;
}
.path input {
  height: 35px;
  width: 43%;
  margin: 3px;
  margin-top: 5px;
  padding: 10px;
  float: right;
}
.path .ok {
  float: right;
  height: 35px;
  margin-top: 5px;
  width: 50px;
  background-color: #fff;
  border: none;
}
.path .ok:hover {
  background-color: rgba(140, 140, 140, 0.1);
}
</style>
