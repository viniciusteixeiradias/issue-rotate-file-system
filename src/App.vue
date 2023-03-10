<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import { Logger } from "tslog";
import { createStream } from "rotating-file-stream";

const stream = createStream("tslog.log", {
  size: "10M", // rotate every 10 MegaBytes written
  interval: "1d", // rotate daily
  compress: "gzip", // compress rotated files
});

const logger = new Logger();
logger.attachTransport((logObj) => {
  stream.write(JSON.stringify(logObj) + "\n");
});

logger.debug("I am a debug log.");
logger.info("I am an info log.");
logger.warn("I am a warn log with a json object:", { foo: "bar" });
console.log("[App.vue]", `Hello world from Electron ${process.versions.electron}!`)
</script>

<template>
  <div>
    <a href="https://www.electronjs.org/" target="_blank">
      <img src="./assets/electron.svg" class="logo electron" alt="Electron logo" />
    </a>
    <a href="https://vitejs.dev/" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Electron + Vite + Vue" />
  <div class="flex-center">
    Place static files into the <code>/public</code> folder
    <img style="width:5em;" src="/node.svg" alt="Node logo">
  </div>
</template>

<style>
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo.electron:hover {
  filter: drop-shadow(0 0 2em #9FEAF9);
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
