<template>
    <div>

        <base-dialog :show="!!error" title="An error occurre" @close="handleError">
            <p>{{ error }}</p>
        </base-dialog>
        <section>
            <base-card>
                <header>
                    <h2>Requests Received</h2>
                </header>
                <base-spinner v-if="isLoading">

                </base-spinner>
                <ul v-else-if="hasRequests && !isLoading">
                    <requests-item v-for="req in receivedRequests " :key="req.id" :email="req.userEmail"
                        :message="req.message"></requests-item>
                </ul>
                <h3 v-else>You havent received any requests yet!</h3>
            </base-card>
        </section>
    </div>
</template>
<script>
import RequestsItem from '../../components/requests/RequestsItem.vue';

export default {
    data() {
        return {
            isLoading: false,
            error: null,
        }
    },
    components: { RequestsItem },
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
                await this.$store.dispatch('requests/fetchRequest');
            } catch (error) {
                this.error = error.message || 'Something Failed!';

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