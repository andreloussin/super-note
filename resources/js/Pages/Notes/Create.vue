<template>
    <AppLayout>
        <template #header>
            <h1 class="text-2xl font-semibold text-gray-800 dark:text-gray-200">
                Create Note
            </h1>
        </template>

        <div class="mt-6 max-w-3xl mx-auto px-4">
            <form @submit.prevent="submitForm" class="space-y-4">
                <!-- Title Input -->
                <div>
                    <label
                        for="title"
                        class="block text-gray-700 dark:text-gray-300"
                        >Title</label
                    >
                    <input
                        id="title"
                        v-model="form.title"
                        type="text"
                        placeholder="Enter the title"
                        class="w-full px-4 py-2 border rounded-md dark:bg-gray-700 dark:border-gray-600 dark:text-gray-200"
                    />
                    <span
                        v-if="form.errors.title"
                        class="text-red-500 text-sm"
                        >{{ form.errors.title }}</span
                    >
                </div>

                <!-- Content Input -->
                <div>
                    <label
                        for="content"
                        class="block text-gray-700 dark:text-gray-300"
                        >Content</label
                    >
                    <textarea
                        id="content"
                        v-model="form.content"
                        rows="10"
                        placeholder="Enter the content"
                        class="w-full px-4 py-2 border rounded-md dark:bg-gray-700 dark:border-gray-600 dark:text-gray-200"
                    ></textarea>
                    <span
                        v-if="form.errors.content"
                        class="text-red-500 text-sm"
                        >{{ form.errors.content }}</span
                    >
                </div>

                <!-- Submit Button -->
                <button
                    type="submit"
                    class="bg-blue-500 text-white px-4 py-2 rounded shadow hover:bg-blue-600"
                >
                    Save Note
                </button>
            </form>
        </div>
    </AppLayout>
</template>

<script>
import AppLayout from "@/Layouts/AuthenticatedLayout.vue";

export default {
    components: {
        AppLayout,
    },

    data() {
        return {
            form: this.$inertia.form({
                title: null,
                content: "",
            }),
        };
    },

    methods: {
        submitForm() {
            this.form.post(route("notes.store"), {
                onSuccess: () => {
                    this.form.reset();
                },
            });
        },
    },
};
</script>

<style scoped>
/* Custom styles can be added here if needed */
</style>
