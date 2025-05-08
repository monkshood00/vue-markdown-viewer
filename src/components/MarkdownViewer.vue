<template>
  <div class="markdown-body" v-html="htmlContent"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { marked } from 'marked'

const htmlContent = ref('')

onMounted(async () => {
  const res = await fetch('/posts/sample.md')
  const md = await res.text()
  htmlContent.value = marked(md)
})
</script>

<style>
.markdown-body {
  max-width: 700px;
  margin: 2rem auto;
  line-height: 1.6;
  font-family: sans-serif;
}
</style>
