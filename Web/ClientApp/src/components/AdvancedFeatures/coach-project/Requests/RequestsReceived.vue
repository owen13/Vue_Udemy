<template>
    <div>
        <base-dialog2 :show="!!error" title="An error occurred!" @close="handleError">
            <p>{{ error }}</p>
        </base-dialog2>
        <section>
            <base-card>
                <h2>Requests Received</h2>
                <div v-if="isLoading">
                    <base-spinner></base-spinner>
                </div>
                <ul v-else-if="hasRequests && !isLoading">
                    <request-item v-for="req in receivedRequests" :key="req.id" :email="req.userEmail"
                                  :message="req.message"></request-item>
                </ul>
                <h3 v-else>You haven't received any requests yet!</h3>
            </base-card>
        </section>
    </div>
</template>

<script>
import RequestItem from "./RequestItem";

export default {
    name: "RequestsReceived",
    components: {RequestItem},
    data() {
        return {
            isLoading: false,
            error: null,
        }
    },
    computed: {
        receivedRequests() {
            return this.$store.getters['requests/requests'];
        },
        hasRequests() {
            return this.$store.getters['requests/hasRequests'];
        }
    },
    created() {
        this.loadRequests();
    },
    methods: {
        async loadRequests() {
            this.isLoading = true;

            try {
                await this.$store.dispatch('requests/fetchRequests');
            } catch (error) {
                this.error = error.message || 'Something went wrong!';
            }

            this.isLoading = false;
        },
        handleError() {
            this.error = null;
        }
    }
}
</script>

<style scoped>
header {
    text-align: center;
}

ul {
    list-style: none;
    margin: 2rem auto;
    padding: 0;
    max-width: 30rem;
}

h3 {
    text-align: center;
}
</style>