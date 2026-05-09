<script setup>
    import { ref, computed, watch } from 'vue'

    const config = useRuntimeConfig()
    const category = ref('')

    const url = computed(() =>
      `${config.public.strapiUrl}/api/blog-posts` +
      (category.value ? `?filters[category][$eq]=${category.value}` : '')
    )

    const { data, pending, error } = useFetch(url, {
      server: false,
      watch: [category]
    })

    const posts = computed(() => data.value?.data ?? [])

    console.log("STRAPI URL:", config.public.strapiUrl)
</script>

<template>
  <div>
    <h1>Blog Homepage</h1>

    <select v-model="category" class="category-select">
      <option value="">All</option>
      <option value="Tech">Tech</option>
      <option value="Life">Life</option>
      <option value="Travel">Travel</option>
    </select>

    <p v-if="pending">Loading...</p>
    <p v-else-if="error && posts.length === 0">Failed to load posts</p>

    <div v-for="post in posts" :key="post.id" class="post-card">

        <h2>
            <NuxtLink :to="`/blog/${post.id}`">
            {{ post.title }}
            </NuxtLink>
        </h2>

        <p><b>Author:</b> {{ post.author }}</p>
        <p>{{ post.snippet }}</p>

    </div>
  </div>
</template>