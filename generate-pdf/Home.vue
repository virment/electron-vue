<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App !!"/>
     <button id='print-pdf' @click="createPDF()">Convert to PDF</button>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import HelloWorld from '@/components/HelloWorld.vue' // @ is an alias to /src
import { ipcRenderer } from 'electron'

@Component({
  components: {
    HelloWorld
  }
})
export default class Home extends Vue {
  pdfname: string = "mypdf"
  
  createPDF () {
    ipcRenderer.send('print-to-pdf', this.pdfname)
  }
  mounted () {
    ipcRenderer.on('wrote-pdf', (event: any, path: any) => {
      const msg = `PDFを ${path} に作成しました。`
      console.log(msg)
    })
  }
}
</script>
