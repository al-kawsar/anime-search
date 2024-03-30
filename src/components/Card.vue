<script setup>
defineProps({
    src: String,
    alt: String,
});
</script>

<template>
    <article class="card">
        <img :src="src" :alt="alt" />
    </article>
</template>

<style scoped>
.card {
    border-radius: 8px;
    display: flex;
    overflow: hidden;
}

img {
    width: 100%;
}
.card img {
    aspect-ratio: 9/16;
    object-fit: cover;
    object-position: center;
}

.blur-image {
    filter: blur(10px);
    transition: filter 0.5s ease;
}

.blur-image.visible {
    filter: none;
}
</style>
