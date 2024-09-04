<template>
    <nav v-if="links.length > 3" class="mt-6">
        <ul class="flex justify-center space-x-2">
            <li v-for="(link, index) in links" :key="index">
                <!-- eslint-disable vue/no-v-html, vue/no-v-text-v-html-on-component -->

                <!-- Use Inertia Link for pagination -->
                <Link
                    v-if="link.url"
                    :href="link.url"
                    :class="[
                        'px-3 py-1 border rounded transition-colors duration-200',
                        link.active
                            ? 'bg-blue-500 text-white border-blue-500'
                            : 'bg-white text-blue-500 border-gray-300 hover:bg-gray-100',
                        'dark:bg-gray-800 dark:text-blue-400 dark:border-gray-600 dark:hover:bg-gray-700',
                        link.active && 'dark:bg-blue-600 dark:border-blue-600',
                    ]"
                    v-html="link.label"
                />
                <span
                    v-else
                    class="px-3 py-1 text-gray-500 dark:text-gray-400 transition-colors duration-200"
                    v-html="link.label"
                ></span>
                <!-- eslint-enable vue/no-v-html, vue/no-v-text-v-html-on-component -->
            </li>
        </ul>
    </nav>
</template>

<script>
import { Link } from "@inertiajs/vue3";

export default {
    components: {
        Link,
    },

    props: {
        links: Array,
    },
};
</script>
