<template>
  <div>
    <v-progress-linear :indeterminate="loading" v-show="loading" class="v-progress-bar" style="z-index: 1000"></v-progress-linear>
    <v-toolbar fixed>
      <v-btn @click="goBack">戻る</v-btn>
      <v-btn @click="goForward">進む</v-btn>
    </v-toolbar>
    <webview id="webview" src="https://app.misoca.jp/invoices"
             style="display:inline-flex; width:1000px; height:563px" ref="webview"></webview>
  </div>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {
      loading: false
    }
  },
  methods: {
    goBack () {
      this.$refs.webview.canGoBack() && this.$refs.webview.goBack()
    },
    goForward () {
      this.$refs.webview.canGoForward() && this.$refs.webview.goForward()
    }
  },
  mounted () {
    this.$refs.webview.addEventListener('did-start-loading', () => { this.loading = true })
    this.$refs.webview.addEventListener('did-stop-loading', () => { this.loading = false })
  }
}
</script>

<style scoped>
  .v-progress-bar {
    position: fixed;
    margin: 0;
  }
  #webview {
    margin-top: 65px;
  }
</style>
