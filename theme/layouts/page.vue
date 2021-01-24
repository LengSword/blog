<template>
  <Wrap :page="page">
    <article class="post">
      <header class="post-header">
        <h1 class="post-title">{{ page.title }}</h1>
      </header>

      <div class="post-content">
        <slot name="default"></slot>
      </div>

      <Utterances
        v-if="page.comments !== false && $themeConfig.utterances"
        :repo="$themeConfig.utterances.repo"
        :theme="$themeConfig.utterances.theme"
      />

    </article>
  </Wrap>
</template>

<script>
import Wrap from '../components/Wrap.vue'
import Utterances from '../components/Utterances.vue'

export default {
  components: {
    Wrap,
    Utterances
  },

  props: ['page']
}
</script>
