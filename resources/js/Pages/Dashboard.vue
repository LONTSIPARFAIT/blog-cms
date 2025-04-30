<script setup>
import { Head, Link } from '@inertiajs/vue3';

defineProps({
    canLogin: Boolean,
    canRegister: Boolean,
    laravelVersion: String,
    phpVersion: String,
    posts: Object,
});

const truncate = (text, length) => {
    const stripped = text.replace(/<[^>]+>/g, '');
    return stripped.length > length ? stripped.substring(0, length) + '...' : stripped;
};
</script>

<template>
    <Head title="Bienvenue" />
    <div class="min-h-screen bg-gray-100 dark:bg-gray-900">
        <header class="bg-white dark:bg-gray-800 shadow">
            <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8 flex justify-between items-center">
                <h1 class="text-3xl font-bold text-gray-900 dark:text-white">Blog CMS</h1>
                <nav v-if="canLogin" class="flex space-x-4">
                    <Link
                        v-if="$page.props.auth.user"
                        :href="route('dashboard')"
                        class="text-gray-600 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400"
                    >
                        Tableau de bord
                    </Link>
                    <template v-else>
                        <Link
                            :href="route('login')"
                            class="text-gray-600 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400"
                        >
                            Se connecter
                        </Link>
                        <Link
                            v-if="canRegister"
                            :href="route('register')"
                            class="text-gray-600 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400"
                        >
                            S'inscrire
                        </Link>
                    </template>
                </nav>
            </div>
        </header>
        <main>
            <div class="max-w-4xl mx-auto py-6 sm:px-6 lg:px-8">
                <h2 class="text-2xl font-bold text-gray-900 dark:text-white mb-4">Articles récents</h2>
                <div v-if="posts.data.length === 0" class="text-gray-500 dark:text-gray-400">
                    Aucun article publié pour le moment.
                </div>
                <div v-else>
                    <div v-for="post in posts.data" :key="post.id" class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow mb-4">
                        <h3 class="text-xl font-semibold text-gray-900 dark:text-white">{{ post.title }}</h3>
                        <p class="text-gray-600 dark:text-gray-300">
                            Par {{ post.user.name }} | {{ new Date(post.created_at).toLocaleDateString('fr-FR') }}
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
        </main>
        <footer class="py-16 text-center text-sm text-gray-500 dark:text-gray-400">
            Laravel v{{ laravelVersion }} (PHP v{{ phpVersion }})
        </footer>
    </div>
</template>