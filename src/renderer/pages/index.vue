<template>
  <div>
    <v-progress-linear :indeterminate="loading" class="webview-progress-bar"></v-progress-linear>
    <v-toolbar fixed class="webview-toolbar" height="50">
      <v-btn @click="goBack" icon>
        <v-icon>arrow_back</v-icon>
      </v-btn>
      <v-btn @click="goForward" icon>
        <v-icon>arrow_forward</v-icon>
      </v-btn>
      <v-btn @click="reload" icon>
        <v-icon>refresh</v-icon>
      </v-btn>
      <v-btn @click="goHome" icon>
        <v-icon>home</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-text-field
          v-model="url"
          @keypress.enter="loadUrl"
      ></v-text-field>
      <v-btn @click="loadUrl" icon>
        <v-icon>loop</v-icon>
      </v-btn>
    </v-toolbar>
    <webview id="webview" src="https://app.misoca.jp/invoices" ref="webview" allowpopups></webview>
  </div>
</template>

<script>
export default { name: 'index',
  data () {
    return {
      loading: false,
      url: '',
      webview: ''
    }
  },
  methods: {
    goBack () {
      this.webview.canGoBack() && this.webview.goBack()
    },
    goForward () {
      this.webview.canGoForward() && this.webview.goForward()
    },
    goHome () {
      this.webview.loadURL()
    },
    reload () {
      this.webview.reload()
    },
    loadUrl () {
      this.url.match(/^https?:\/\//) ? this.webview.loadURL(this.url)
        : this.webview.loadURL(`http://${this.url}`)
    },
    setUrlBar (event) {
      if (event.isMainFrame) {
        this.url = event.url
      }
    }
  },
  mounted () {
    // webviewの取得
    this.webview = document.getElementById('webview')

    // loading Eventの付与
    this.webview.addEventListener('did-start-loading', () => {
      this.loading = true
    })
    this.webview.addEventListener('did-stop-loading', () => {
      this.loading = false
    })

    // commit Eventの付与
    this.webview.addEventListener('load-commit', (e) => {
      this.setUrlBar(e)
    })

    // 初期URLの設定
    this.url = 'https://app.misoca.jp/invoices'
  }
}
</script>

<style scoped lang="scss">
  .webview-progress-bar {
    position: fixed;
    margin: 0;
    z-index: 999999;
  }

  #webview {
    margin-top: 57px;
    display: inline-flex;
    width: 1000px;
    height: 563px
  }

  .webview-toolbar {
    margin-top: 7px !important;
  }
</style>
