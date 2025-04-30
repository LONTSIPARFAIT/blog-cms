<template>
    <div class="min-h-screen bg-gray-100">
      <header class="bg-white shadow">
        <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
          <h1 class="text-3xl font-bold text-gray-900">Bienvenue sur Blog CMS</h1>
        </div>
      </header>
      <main>
        <div class="max-w-4xl mx-auto py-6 sm:px-6 lg:px-8">
          <div class="mb-6" v-if="canLogin || canRegister">
            <inertia-link
              v-if="canLogin"
              :href="route('login')"
              class="mr-4 text-indigo-600 hover:text-indigo-800"
            >
              Se connecter
            </inertia-link>
            <inertia-link
              v-if="canRegister"
              :href="route('register')"
              class="text-indigo-600 hover:text-indigo-800"
            >
              S'inscrire
            </inertia-link>
          </div>
          <h2 class="text-2xl font-bold mb-4">Articles récents</h2>
          <div v-if="posts.data.length === 0" class="text-gray-500">
            Aucun article publié pour le moment.
          </div>
          <div v-else>
            <div v-for="post in posts.data" :key="post.id" class="bg-white p-6 rounded-lg shadow mb-4">
              <h3 class="text-xl font-semibold">{{ post.title }}</h3>
              <p class="text-gray-600">
                Par {{ post.user.name }} | {{ new Date(post.created_at).toLocaleDateString('fr-FR') }}
              </p>
              <p v-html="truncate(post.content, 200)" class="mt-2"></p>
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
      </main>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  defineProps({
    canLogin: Boolean,
    canRegister: Boolean,
    posts: Object,
  });
  
  const truncate = (text, length) => {
    const stripped = text.replace(/<[^>]+>/g, '');
    return stripped.length > length ? stripped.substring(0, length) + '...' : stripped;
  };
  </script>