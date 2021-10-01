<template>
  <div>
    <MonacoEditor
      height="1000"
      width="1200"
      language="text"
      :code="code"
      :editorOptions="options"
      @mounted="onMounted"
      @codeChange="onCodeChange"
    >
    </MonacoEditor>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import MonacoEditor from "vue-monaco-editor";
import { Prop } from "vue-property-decorator";
import { EventEmitter } from "events";
import { getCurrent } from "@tauri-apps/api/window";

@Options({
  components: {
    MonacoEditor,
  },
})
export default class Editor extends Vue {
  public code = "d";
  @Prop() public messageEmitter?: EventEmitter;
  public editor: any;

  created() {
    this.messageEmitter!.on("change-code", (code: string) => {
      this.editor.setValue(code);
    });
  }

  onMounted(editor: any) {
    this.editor = editor;
    getCurrent().listen("test", (x: any) => {
      this.editor.setValue(x.payload);
    });
  }
}
</script>

<style scoped>
</style>
