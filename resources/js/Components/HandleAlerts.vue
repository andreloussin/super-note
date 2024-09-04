<template>
    <Teleport to="body">
        <div
            v-if="alerts.length"
            class="fixed top-0 left-1/2 transform -translate-x-1/2 w-full max-w-md mt-4 z-50"
        >
            <template v-for="customAlert in alerts" :key="customAlert.id">
                <div
                    v-if="customAlert.success"
                    class="p-4 bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200 rounded shadow mb-4"
                    @click.prevent="removeAlert(customAlert.id, 0)"
                >
                    {{ customAlert.success }}
                </div>
                <div
                    v-else-if="customAlert.error"
                    class="p-4 bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-200 rounded shadow mb-4"
                    @click.prevent="removeAlert(customAlert.id, 0)"
                >
                    {{ customAlert.error }}
                </div>
            </template>
        </div>
    </Teleport>
</template>

<script>
import { usePage } from "@inertiajs/vue3";
import { v4 as uuidv4 } from "uuid";
import { ref, computed, watch } from "vue";

export default {
    data() {
        return {
            alerts: ref([]),
            MAX_NOTIFICATIONS: 3,
            DEFAULT_DURATION: 3000,
        };
    },
    computed: {
        flash() {
            return computed(() => usePage().props.flash);
        },
    },
    watch: {
        flash: {
            handler(newVal) {

                if (newVal.value) {
                    this.addAlert(newVal.value);
                }
            },
            deep: true,
        },
    },
    methods: {
        removeAlert(id, duration = null) {
            if (null === duration) {
                duration = this.DEFAULT_DURATION;
            }

            setTimeout(() => {
                this.alerts = this.alerts.filter((alert) => alert.id !== id);
            }, duration);
        },

        addAlert(alert) {
            const id = uuidv4();

            this.alerts.push({
                id: id,
                ...alert,
            });

            this.removeAlert(id, alert.duration);

            if (this.alerts.length > this.MAX_NOTIFICATIONS) {
                this.alerts = this.alerts.slice(1);
            }
        },
    },
};
</script>
