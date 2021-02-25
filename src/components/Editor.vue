<template>
    <div class="toolbar">
        <div class="controls">
            <div class="close"></div>
            <div class="shrink"></div>
            <div class="expand"></div>
        </div>
        <div class="filename">
            <input v-model="filename"/>
        </div>
    </div>
    <div>
        <div id="root" ref="root"></div>
        <button @click="exportSVG">Export</button>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from "vue";
import * as monaco from 'monaco-editor'
import EditorWorker from 'monaco-editor/esm/vs/editor/editor.worker?worker'
import TsWorker from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker'

// @ts-ignore
self.MonacoEnvironment = {
  getWorker(_: string, label: string) {
    if (['typescript', 'javascript'].includes(label)) {
      return new TsWorker()
    }
    return new EditorWorker()
  }
}
export default defineComponent({
  setup(_props, { emit }) {
    const root = ref<HTMLElement>()
    const filename = ref<string>('untitled-1')
    let editor: monaco.editor.IStandaloneCodeEditor
    onMounted(() => {
      editor = monaco.editor.create(root.value as HTMLElement, {
        language: 'typescript',
        value: ``,
        theme: 'vs-dark',
        minimap: {
            enabled: false
        },
        lineNumbers: 'off',
        fontSize: 20
      })
    })
    onUnmounted(() => {
      editor.dispose()
    })

    function exportSVG () {
      emit('export', {
        filename: filename.value,
        // @ts-ignore
        code: editor._modelData.viewModel.getHTMLToCopy([editor.getModel().getFullModelRange()])
      })
    }

    return {
      root,
      filename,
      exportSVG
    }
  }
})
</script>

<style scoped>
#root {
  width: 100%;
  height: 100vh;
}

.toolbar {
  background-color: #EEF1F5;
  height: 30px;
  border-radius: 10px 10px 0 0;
  display: flex;
  flex-direction: row;
}

.toolbar .controls {
  flex-shrink: 1;
}

.controls div {
  width: 12px;
  height: 12px;
  margin-top: 10px;
  margin-left: 10px;
  border-radius: 50%;
  display: inline-block;
}

.controls .close {
  background: #EF534A;
}

.controls .shrink {
  background: #EDC04A;
}

.controls .expand {
  background: #97D272;
}

.toolbar .filename {
  flex-grow: 1;
  text-align: center;
  margin-top: 5px;
}

.toolbar input{
  background-color: transparent;
  border: none
}
</style>