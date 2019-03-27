<template>
  <div>
    <v-progress-linear :indeterminate="loading" class="v-progress-bar"></v-progress-linear>
    <v-toolbar fixed class="v-toolbar">
      <v-btn @click="goBack" icon>
        <v-icon>arrow_back</v-icon>
      </v-btn>
      <v-btn @click="goForward" icon>
        <v-icon>arrow_forward</v-icon>
      </v-btn>
      <v-btn @click="reload" icon>
        <v-icon>loop</v-icon>
      </v-btn>
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
    },
    reload () {
      this.$refs.webview.reload()
    }
  },
  mounted () {
    this.$refs.webview.addEventListener('did-start-loading', () => { this.loading = true })
    this.$refs.webview.addEventListener('did-stop-loading', () => { this.loading = false })
  }
}
</script>

<style scoped lang="scss">
  .v-progress-bar {
    position: fixed;
    margin: 0;
    z-index: 100;
  }
  #webview {
    margin-top: 57px;
  }
  .v-toolbar {
    margin-top: 7px;
    .v-toolbar__content {
      height: 50px;
    }
  }
</style>
