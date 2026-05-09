<script setup>
    import { ref } from 'vue'

    const config = useRuntimeConfig()
    const query = ref('')
    const posts = ref([])

    const search = async () => {
      if (!query.value) return

      const url = `${config.public.strapiUrl}/api/blog-posts?filters[$or][0][title][$contains]=${query.value}&filters[$or][1][author][$contains]=${query.value}`

      const res = await $fetch(url)
      posts.value = res.data ?? []
    }
    
</script>

<template>
  <div>
    <h1>Search</h1>

    <input v-model="query" placeholder="Search posts or authors..." />

    <button @click="search">Search</button>

    <div v-for="post in posts" :key="post.id" class="post-card">

      <NuxtLink :to="`/blog/${post.id}`">
        <h3>{{ post.title }}</h3>
      </NuxtLink>

      <p>{{ post.author }}</p>
    </div>

  </div>
</template>