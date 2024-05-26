<script setup>
defineProps({
    src: String,
    title: String,
});
</script>

<template>
    <article class="card">
        <img :src="src" :alt="title" />
        <h1>{{ title }}</h1>
    </article>
</template>

<style scoped>
.card {
    display: flex;
    flex-direction: column;
}

img {
    width: 100%;
}
.card img {
    aspect-ratio: 9/16;
    object-fit: cover;
    object-position: center;
    border-radius: 8px;
}
</style>
