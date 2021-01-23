<template>
  <div class="comments" ref="comments"></div>
</template>

<script>
export default {
  props: {
    repo: {
      type: String,
      required: true
    },
    theme: {
      type: String,
      required: false
    }
  },

  mounted() {
    const { repo, theme } = this
    const attrs = {
      repo: repo,
      'issue-term': 'pathname',
      label: 'comment',
      theme: theme,
      crossorigin: 'anonymous'
    }

    const script = (this.$commentScript = document.createElement('script'))
    script.async = true
    script.src = `https://utteranc.es/client.js`
    for (const key of Object.keys(attrs)) {
      script.setAttribute(key, attrs[key])
    }
    this.$refs.comments.append(script)
  },

  beforeDestroy() {
    if (this.$commentScript) {
      this.$commentScript.parentNode?.removeChild(this.$commentScript)
    }
  }

}
</script>

<style scoped>
.comments {
  margin-top: 50px;
  padding-top: 50px;
  border-top: 1px dashed hsla(0,0%,100%,0.25);
}
</style>
