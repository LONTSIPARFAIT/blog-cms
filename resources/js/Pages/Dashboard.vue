<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link } from '@inertiajs/vue3';

defineProps({
    posts: Object,
});

const truncate = (text, length) => {
    const stripped = text.replace(/<[^>]+>/g, '');
    return stripped.length > length ? stripped.substring(0, length) + '...' : stripped;
};
</script>

<template>
    <Head title="Tableau de bord" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold leading-tight text-gray-800 dark:text-gray-200">
                Tableau de bord
            </h2>
        </template>

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="overflow-hidden bg-white dark:bg-gray-800 shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900 dark:text-gray-100">
                        <div class="mb-6">
                            <Link
                                :href="route('posts.create')"
                                class="bg-indigo-600 text-white px-4 py-2 rounded-md hover:bg-indigo-700 dark:hover:bg-indigo-500"
                            >
                                Créer un nouvel artic
                            </Link>
                        </div>
                        <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Mes articles</h3>
                        <div v-if="posts.data.length === 0" class="text-gray-500 dark:text-gray-400">
                            Vous n'avez aucun article pour le moment.
                        </div>
                        <div v-else>
                            <div v-for="post in posts.data" :key="post.id" class="bg-gray-50 dark:bg-gray-700 p-4 rounded-md mb-4">
                                <h4 class="text-md font-semibold text-gray-900 dark:text-white">{{ post.title }}</h4>
                                <p class="text-gray-600 dark:text-gray-300">
                                    Statut : {{ post.status === 'draft' ? 'Brouillon' : 'Publié' }} |
                                    {{ new Date(post.created_at).toLocaleDateString('fr-FR') }}
                                </p>
                                <p v-html="truncate(post.content, 200)" class="mt-2 text-gray-700 dark:text-gray-200"></p>
                            </div>
                            <div class="mt-4 flex justify-center">
                                <Link
                                    v-for="page in posts.links"
                                    :key="page.label"
                                    :href="page.url"
                                    class="px-2 py-1 mx-1 rounded"
                                    :class="{ 'bg-indigo-600 text-white': page.active, 'text-gray-600 dark:text-gray-300': !page.active }"
                                    v-html="page.label"
                                ></Link>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>