<template>
    <section>
        <base-card>
            <template #header>
                <h3>{{ fullName }}</h3>
                <base-badge :type="role" :text="role.toUpperCase()"></base-badge>
            </template>
            <p>{{ infoText }}</p>
        </base-card>
    </section>
</template>

<script>
export default {
    props: ['fullName', 'infoText', 'role'],
};
</script>

<style scoped>
    section p {
        text-align: left;
    }
</style>