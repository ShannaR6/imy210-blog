<script setup>
import { useRoute } from 'vue-router'
import MarkdownIt from 'markdown-it'
import { computed } from 'vue'

const config = useRuntimeConfig()
const route = useRoute()
const md = new MarkdownIt()

const { data, pending, error } = await useFetch(
  `${config.public.strapiUrl}/api/blog-posts?filters[id][$eq]=${route.params.id}`
)

const post = computed(() => data.value?.data?.[0] ?? null)
</script>

<template>
  <div class="post-page">

    <p v-if="pending">Loading...</p>
    <p v-if="error">Post not found</p>

    <div v-if="post">

      <h1>{{ post.title }}</h1>

      <p class="author">
        <b>Author:</b> {{ post.author }}
      </p>

      <hr />

      <!-- Markdown content -->
      <div
        class="markdown-content"
        v-html="md.render(post.content)"
      ></div>

    </div>

  </div>
</template>