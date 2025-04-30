<template>
    <div class="max-w-4xl mx-auto p-4">
      <h1 class="text-2xl font-bold mb-4">Articles</h1>
      <div v-if="$page.props.flash.success" class="bg-green-100 text-green-700 p-4 mb-4 rounded">
        {{ $page.props.flash.success }}
      </div>
      <div v-if="posts.data.length === 0" class="text-gray-500">
        Aucun article publié pour le moment.
      </div>
      <div v-else>
        <div v-for="post in posts.data" :key="post.id" class="border-b py-4">
          <h2 class="text-xl font-semibold">{{ post.title }}</h2>
          <p class="text-gray-600">Par {{ post.user.name }} | {{ new Date(post.created_at).toLocaleDateString('fr-FR') }}</p>
          <p v-html="truncate(post.content, 200)"></p>
        </div>
        <div class="mt-4 flex justify-center">
          <inertia-link
            v-for="page in posts.links"
            :key="page.label"
            :href="page.url"
            class="px-2 py-1 mx-1 rounded"
            :class="{ 'bg-indigo-600 text-white': page.active, 'text-gray-600': !page.active }"
            v-html="page.label"
          ></inertia-link>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  defineProps({
      posts: Object,
  });
  
  const truncate = (text, length) => {
      const stripped = text.replace(/<[^>]+>/g, '');
      return stripped.length > length ? stripped.substring(0, length) + '...' : stripped;
  };
  </script>