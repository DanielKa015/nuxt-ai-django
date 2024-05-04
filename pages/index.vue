<template>
  <div class="container">
    <div class="search-container">
      <input type="text" v-model="searchQuery" placeholder="Wyszukaj obrazy...">
      <button @click="searchData" :disabled="loader">{{ loader ? 'Wyszukiwanie...' : 'Wyszukaj' }}</button>
    </div>
    <div class="loading" v-if="loader">Trwa ładowanie danych...</div>

    <div class="results" v-else-if="arrayData && arrayData.length > 0">
      <div class="image-card" v-for="(item, index) in arrayData" :key="index">
        <div class="image-overlay">
          <p class="image-description">{{ item.alt_description }}</p>
        </div>
        <img :src="item.urls.regular" :alt="item.alt_description">
      </div>
    </div>
    <div v-else>
      <p class="no-results">Brak wyników wyszukiwania dla tej frazy.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const searchQuery = ref("star");
const resultQuery = ref(null);
const arrayData = ref(null);
const loader = ref(false);

const searchData = async () => {
  loader.value = true;
  const response = await fetch(`https://api.unsplash.com/search/photos?query=${searchQuery.value}&client_id=zZKclMyxDzIt4CNElsfrMAMJf1VuggsSmofbr1of3iw`);
  const data = await response.json();
  resultQuery.value = data.results;
  loader.value = false;
};

onBeforeMount(async () => {
  await searchData();
});

watch(resultQuery, (newValue, oldValue) => {
  arrayData.value = newValue;
}, {
  deep: true
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.search-container {
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 70%;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.loading {
  text-align: center;
  font-size: 18px;
  margin-top: 20px;
}

.results {
  margin-top: 20px;
}

.image-card {
  margin-bottom: 20px;
  position: relative; /* Dodajemy pozycję względną dla kontenera obrazka */
}

.image-overlay {
  position: absolute; /* Ustawiamy pozycję absolutną dla nakładki z tekstem */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5); /* Dodajemy przezroczyste tło */
  display: flex;
  justify-content: center;
  align-items: center;
}

img {
  max-width: 100%;
  height: auto;
}

.image-description {
  color: #fff;
  font-size: 16px;
  text-align: center;
}

.no-results {
  text-align: center;
  font-size: 18px;
  color: #777;
}
</style>
