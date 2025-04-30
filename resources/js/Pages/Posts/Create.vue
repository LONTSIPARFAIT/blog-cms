<template>
    <div class="max-w-4xl mx-auto p-4">
      <h1 class="text-2xl font-bold mb-4">Créer un article</h1>
      <form @submit.prevent="handleSubmit">
        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-700">Titre</label>
          <input
            v-model="form.title"
            type="text"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500"
          />
          <span v-if="form.errors.title" class="text-red-500 text-sm">{{ form.errors.title }}</span>
        </div>
        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-700">Contenu</label>
          <input id="trix-content" type="hidden" v-model="form.content">
          <trix-editor input="trix-content" @trix-change="handleContentChange"></trix-editor>
          <span v-if="form.errors.content" class="text-red-500 text-sm">{{ form.errors.content }}</span>
        </div>
        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-700">Statut</label>
          <select
            v-model="form.status"
            class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500"
          >
            <option value="draft">Brouillon</option>
            <option value="published">Publié</option>
          </select>
        </div>
        <div class="mt-4">
          <h2 class="text-lg font-semibold">Aperçu</h2>
          <div class="prose border p-4" v-html="preview"></div>
        </div>
        <button
          type="submit"
          class="mt-4 bg-indigo-600 text-white px-4 py-2 rounded-md hover:bg-indigo-700"
          :disabled="form.processing"
        >
          Créer
        </button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { useForm } from '@inertiajs/vue3';
  import { ref } from 'vue';
  
  const form = useForm({
      title: '',
      content: '',
      status: 'draft',
  });
  
  const preview = ref('');
  
  const handleContentChange = (event) => {
      form.content = event.target.value;
      preview.value = form.content;
  };
  
  const handleSubmit = () => {
      form.post(route('posts.store'), {
          preserveScroll: true,
          onSuccess: () => {
              form.reset();
              preview.value = '';
          },
          onError: (errors) => {
              console.error(errors);
          },
      });
  };
  </script>
  
  <style>
  @import 'trix/dist/trix.css';
  .prose {
      max-width: none;
  }
  </style>