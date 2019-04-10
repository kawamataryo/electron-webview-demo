<template>
  <TheWebview init-url="https://app.misoca.jp/sessions/new"></TheWebview>
</template>

<script>
import TheWebview from '../components/TheWebview'

export default {
  name: 'index',
  components: {
    TheWebview
  }
}
</script>

