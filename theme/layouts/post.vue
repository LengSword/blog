<template>
  <Wrap :page="page">
    <article class="post h-entry" itemscope itemtype="http://schema.org/BlogPosting">
      <header class="post-header">
        <h1 class="post-title p-name" itemprop="name headline">{{ page.title }}</h1>
        <p class="post-meta">
          Created on <time
            class="dt-published"
            :datetime="page.createdAt"
            itemprop="datePublished"
          >{{ formatDate(page.createdAt) }}</time> |
          Recently updated on <time
            class="dt-updated"
            :datetime="page.updatedAt"
            itemprop="dateUpdated"
          >{{ formatDate(page.updatedAt) }}</time>
        </p>
      </header>

      <div class="post-content e-content" itemprop="articleBody">
        <slot name="default"/>
      </div>

      <div
        class="pagination"
        v-if="page.prevPost || page.nextPost"
      >
        <saber-link
          class="prev-link"
          :to="page.prevPost.permalink"
          :title="`Older Post: ${page.prevPost.title}`"
          v-if="page.prevPost"
        >&LeftArrow; {{ page.prevPost.title }}</saber-link>
        <saber-link
          class="next-link"
          :to="page.nextPost.permalink"
          :title="`Newer Post: ${page.nextPost.title}`"
          v-if="page.nextPost"
        >{{ page.nextPost.title }} &RightArrow;</saber-link>
      </div>

      <Utterances
        v-if="page.comments !== false && $themeConfig.utterances"
        :repo="$themeConfig.utterances.repo"
        :theme="$themeConfig.utterances.theme"
      />

      <a class="u-url" :href="page.permalink" hidden></a>
    </article>
  </Wrap>
</template>

<script>
import formatDate from '../utils/formatDate'
import Wrap from '../components/Wrap.vue'
import Utterances from '../components/Utterances.vue'

export default {
  components: {
    Wrap,
    Utterances
  },

  props: ['page'],

  methods: {
    formatDate
  }
}
</script>
