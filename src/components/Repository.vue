<template>
  <div class="repository">
    <input type="string" @keyup.enter="updatePath" v-model.lazy="path" />
    <div v-for="item in files" :key="item.path">
      <span
        class="item"
        @click.left.exact="OpenFile(item)"
        @click.shift="OpenInNewTab(item)"
        >{{ item.name }}</span
      >
    </div>
  </div>
</template>

<script lang="ts">
import { Vue } from "vue-class-component";
import { FileEntry, readDir, readTextFile } from "@tauri-apps/api/fs";
import { WebviewWindow } from "@tauri-apps/api/window";
import { EventEmitter } from "events";
import { Prop } from "vue-property-decorator";

export default class Repository extends Vue {
  public files: FileEntry[] = [];
  @Prop() public messageEmitter?: EventEmitter;

  public path = "C:\\Users\\dan.chiche";
  async created() {
    this.files = await readDir(this.path);
  }

  async OpenFile(item: FileEntry) {
    if (item.children != null) {
      this.files = await readDir(item.path);
    } else {
      const code = await readTextFile(item.path);
      this.messageEmitter!.emit("change-code", code);
    }
  }

  async OpenInNewTab(item: FileEntry) {
    if (item.children == null) {

     const code = await readTextFile(item.path);
      let newWebView = new WebviewWindow("ZA WORLD", {
        url: "public/index.html",
      });

      newWebView.show();

      setTimeout(async () => {
        let newWebView2 = new WebviewWindow("ZA WORLD", {
          url: "public/index.html",
        });

        await newWebView2.emit("test", code);
      }, 1000);
    }
  }

  async updatePath() {
    this.files = await readDir(this.path);
  }
}
</script>

<style scoped>
.repository {
  background: #1e1e1e;
  color: white;
  border: white solid 1px;
  height: 1000px;
  width: 1900px;
  overflow: scroll;
}

.item:hover {
  background: white;
  color: black;
  cursor: pointer;
}
</style>
