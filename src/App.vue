<script setup>
import { ref, watch, onMounted } from "vue";
import Card from "./components/Card.vue";
import Spinner from "./components/icons/Spinner.vue";

const animeData = ref([]);
const isLoading = ref(false);
const searchQuery = ref("");
const oldSearchQuery = ref("");
const apiEndpoint = "https://api.jikan.moe/v4/anime";
let searchTimer = null;

const fetchData = async (url) => {
    try {
        isLoading.value = true;
        const options = {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json",
            },
        };

        const response = await fetch(url, options);
        const data = await response.json();
        return data.data;
    } catch (error) {
        console.error("Error fetching data:", error);
        return [];
    } finally {
        isLoading.value = false;
    }
};

const searchAnime = async (query) => {
    const searchUrl = `${apiEndpoint}?q=${query}`;
    animeData.value = await fetchData(searchUrl);
};

const getAllAnime = async () => {
    animeData.value = await fetchData(apiEndpoint);
};

const handleSearch = () => {
    clearTimeout(searchTimer);
    searchTimer = setTimeout(async () => {
        if (searchQuery.value.length === 0) {
            isLoading.value = true;
            await getAllAnime();
            console.log("length", searchQuery.value);
        }
        if (searchQuery.value.trim() !== "" && searchQuery.value !== oldSearchQuery.value) {
            isLoading.value = true;
            console.log("trim", searchQuery.value);
            await searchAnime(searchQuery.value);
        }
        isLoading.value = false;
    }, 150);
};

watch(searchQuery, (newValue, oldValue) => {
    oldSearchQuery.value = oldValue;
});

onMounted(() => {
    getAllAnime();
});
</script>

<template>
    <div class="container">
        <header>
            <h1><strong>Anime</strong> Search</h1>

            <form @submit.prevent :disabled="isLoading">
                <input type="text" class="form-field" placeholder="Search your anime..." v-model="searchQuery"
                    autocomplete="" @input="handleSearch" />
            </form>
        </header>
        <main>
            <Spinner v-if="isLoading" />
            <section class="anime-list" v-else-if="animeData.length > 0 && !isLoading">
                <Card v-for="anime in animeData" :key="anime.id" :src="anime.images.webp.image_url" :title="anime.title" />
            </section>
            <section class="no-data" v-else>
                <h1>No data Avalaible</h1>
            </section>
        </main>
    </div>
</template>

<style scoped>
body * {
    font-family: sans-serif;
}

header {
    width: 100%;
    position: sticky;
    left: 0;
    top: 0;
    background-color: white;
    padding: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    z-index: 1;
}

header h1 {
    color: rgba(0, 0, 0, 0.5);
    font-weight: 700;
}

header strong {
    color: rgba(0, 0, 0, 1);
}

form {
    width: 100%;
    display: grid;
    place-items: center;
}

.form-field {
    appearance: none;
    background: none;
    border: none;
    outline: none;

    width: 50%;
    box-shadow: 0 0 0 1px #252525;
    padding: 0.75rem;
    font-size: 20px;
    border-radius: 6px;
    transition: all 0.2s ease;
}

.form-field:focus {
    background-color: #333;
    color: white;
}

.container {
    width: 90%;
    margin: 0 auto;
}

.anime-list {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 4rem;
    margin-top: 2.5rem;
}

.no-data {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 250px;
}
</style>
