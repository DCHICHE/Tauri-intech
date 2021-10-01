<template>
  <div class="container">
      <Repository :messageEmitter="messageEmitter" />
      <Editor :messageEmitter="messageEmitter" />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Repository from "./components/Repository.vue";
import Editor from "./components/Editor.vue";
import { EventEmitter } from "events";

@Options({
  components: {
    Repository,
    Editor,
  },
})
export default class App extends Vue {
    public messageEmitter = new EventEmitter();
}
</script>

<style>
.container {
  display: flex;
  justify-content: space-between;
  overflow: hidden;
}
</style>
