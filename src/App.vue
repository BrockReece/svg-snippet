<template>
  <Editor @export="exportSvg"/>
  <SVGBuilder v-if="showSvg" :code="code" :filename="filename"/>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import Editor from './components/Editor.vue'
import SVGBuilder from './components/SVGBuilder.vue'

interface ExportEvent {
  code: string,
  filename: string
}

export default defineComponent({
  name: 'App',
  components: {
    Editor,
    SVGBuilder
  },

  setup() {
    const showSvg = ref(false)
    const filename = ref<string>('')
    const code = ref<string>('')

    function exportSvg(val: ExportEvent) {
      showSvg.value = true
      code.value = val.code
      filename.value = val.filename
    }

    return {
      exportSvg,
      showSvg,
      filename,
      code
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>