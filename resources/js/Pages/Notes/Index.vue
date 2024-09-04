<template>
    <AppLayout>
        <template #header>
            <h1 class="text-2xl font-semibold text-gray-800 dark:text-gray-200">
                Notes
            </h1>
        </template>

        <div class="mt-6 max-w-3xl mx-auto px-4">
            <!-- Search Input and Add Button -->
            <div
                class="flex flex-col sm:flex-row sm:items-center sm:justify-between mb-4 space-y-2 sm:space-y-0"
            >
                <!-- Add Button on mobile above and on desktop to the right -->
                <div class="flex justify-center sm:justify-start sm:order-2">
                    <Link
                        href="/notes/create"
                        class="bg-blue-500 text-white px-4 py-2 rounded shadow hover:bg-blue-600 flex items-center justify-center text-center"
                    >
                        Add Note
                    </Link>
                </div>

                <input
                    v-model="form.search"
                    type="text"
                    placeholder="Search notes..."
                    class="w-full max-w-sm px-4 py-2 border rounded-md dark:bg-gray-700 dark:border-gray-600 dark:text-gray-200 sm:order-1"
                />
            </div>

            <!-- Notes List -->
            <div v-if="notes.data.length" class="space-y-4">
                <div
                    v-for="note in notes.data"
                    :key="note.id"
                    class="p-4 bg-white rounded shadow dark:bg-gray-800"
                >
                    <h2
                        class="text-lg font-bold text-gray-800 dark:text-gray-100"
                    >
                        {{ note.title }}
                    </h2>
                    <div class="flex space-x-2">
                        <Link
                            :href="`/notes/${note.id}/edit`"
                            class="text-blue-500 hover:underline dark:text-blue-400 dark:hover:underline"
                        >
                            Edit
                        </Link>
                        <Link
                            :href="`/notes/${note.id}`"
                            method="delete"
                            class="text-red-500 hover:underline dark:text-red-400 dark:hover:underline"
                        >
                            Delete
                        </Link>
                        <!-- <button
                            @click="confirmDelete(note.id)"
                            class="text-red-500 hover:underline dark:text-red-400 dark:hover:underline"
                        >
                            Delete
                        </button> -->
                    </div>
                </div>
            </div>

            <!-- No Results Message -->
            <div v-else class="text-center text-gray-500 dark:text-gray-400">
                No notes found.
            </div>

            <!-- Pagination -->
            <Pagination :links="notes.links" />
        </div>
    </AppLayout>
</template>

<script>
import { Link } from "@inertiajs/vue3";
import AppLayout from "@/Layouts/AuthenticatedLayout.vue";
import Pagination from "@/Components/Pagination.vue";
import { throttle, pickBy } from "lodash";

export default {
    components: {
        AppLayout,
        Pagination,
        Link,
    },

    props: {
        notes: {
            type: Object,
            required: true,
        },

        filters: {
            type: Object,
            default: () => ({}),
        },
    },

    data() {
        return {
            form: this.$inertia.form({
                search: this.filters.search ?? null,
            }),
        };
    },

    watch: {
        form: {
            deep: true,
            handler: throttle(function () {
                this.$inertia.get("/notes", pickBy(this.form), {
                    preserveState: true,
                });
            }, 150),
        },
    },
};
</script>

<style scoped>
/* Custom styles can be added here if needed */
</style>
