<script setup>
import { ref, onMounted, computed, watch } from "vue";
import axios from "axios";

const isDarkMode = ref(true);
const characters = ref([]);
const loading = ref(true);
const searchTerm = ref("");
const selectedStatus = ref("");
const selectedSpecies = ref("");

const currentPage = ref(1);
const totalPages = ref(0);
const totalCharacters = ref(0);
const paginationInfo = ref({});

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
};

const applyTheme = (darkMode) => {
  document.body.className = darkMode ? "dark" : "light";
};

const saveThemePreference = (darkMode) => {
  localStorage.setItem("rickAndMortyTheme", darkMode ? "dark" : "light");
};

const loadThemePreference = () => {
  const saved = localStorage.getItem("rickAndMortyTheme");
  if (saved) {
    isDarkMode.value = saved === "dark";
  }
};

watch(isDarkMode, (newValue) => {
  applyTheme(newValue);
  saveThemePreference(newValue);
});

const fetchCharacters = async (page = 1) => {
  loading.value = true;
  try {
    let url = `https://rickandmortyapi.com/api/character?page=${page}`;

    const filters = [];
    if (searchTerm.value)
      filters.push(`name=${encodeURIComponent(searchTerm.value)}`);
    if (selectedStatus.value) filters.push(`status=${selectedStatus.value}`);
    if (selectedSpecies.value) filters.push(`species=${selectedSpecies.value}`);

    if (filters.length > 0) url += `&${filters.join("&")}`;

    const response = await axios.get(url);

    characters.value = response.data.results;
    paginationInfo.value = response.data.info;
    totalPages.value = response.data.info.pages;
    totalCharacters.value = response.data.info.count;
    currentPage.value = page;
  } catch (error) {
    console.error("Error fetching characters:", error);
    characters.value = [];
  } finally {
    loading.value = false;
  }
};

const filteredCharacters = computed(() => characters.value);

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    fetchCharacters(currentPage.value + 1);
    scrollToTop();
  }
};

const prevPage = () => {
  if (currentPage.value > 1) {
    fetchCharacters(currentPage.value - 1);
    scrollToTop();
  }
};

const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    fetchCharacters(page);
    scrollToTop();
  }
};

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

const clearFilters = () => {
  searchTerm.value = "";
  selectedStatus.value = "";
  selectedSpecies.value = "";
  currentPage.value = 1;
  fetchCharacters(1);
};

const getVisiblePages = () => {
  const total = totalPages.value;
  const current = currentPage.value;
  const pages = [];

  let start = Math.max(1, current - 2);
  let end = Math.min(total, current + 2);

  if (current <= 3) {
    end = Math.min(5, total);
  }
  if (current >= total - 2) {
    start = Math.max(1, total - 4);
  }

  for (let i = start; i <= end; i++) {
    pages.push(i);
  }
  return pages;
};

watch([searchTerm, selectedStatus, selectedSpecies], () => {
  currentPage.value = 1;
  fetchCharacters(1);
});

onMounted(() => {
  fetchCharacters();
  loadThemePreference();
  applyTheme(isDarkMode.value);
});
</script>

<template>
  <div class="app">
    <button @click="toggleTheme" class="theme-button">
      {{ isDarkMode ? "☀️" : "🌙" }}
    </button>
    <h1>Rick and Morty Characters</h1>

    <input v-model="searchTerm" type="text" placeholder="Search character..." />

    <div class="filters">
      <select v-model="selectedStatus" placeholder="Select status">
        <option value="">All</option>
        <option value="Alive">Alive</option>
        <option value="Dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>

      <select v-model="selectedSpecies" placeholder="Select species">
        <option value="">All</option>
        <option value="Human">Human</option>
        <option value="Alien">Alien</option>

        <option value="Animal">Animal</option>
        <option value="Unknown">Unknown</option>
      </select>

      <button @click="clearFilters">Clear Filters</button>
    </div>
    <div v-if="loading">Loading...</div>

    <div v-else class="characters-grid">
      <div
        v-for="character in filteredCharacters"
        :key="character.id"
        class="character-card"
      >
        <img :src="character.image" :alt="character.name" />
        <h3>{{ character.name }}</h3>
        <p>{{ character.species }} - {{ character.status }}</p>
      </div>
    </div>

    <div v-if="!loading && characters.length > 0" class="pagination">
      <div class="pagination-info">
        Página {{ currentPage }} de {{ totalPages }} ({{ totalCharacters }}
        personagens)
      </div>

      <div class="pagination-controls">
        <button @click="prevPage" :disabled="currentPage === 1">
          <- Anterior
        </button>

        <div class="page-numbers">
          <button
            v-for="page in getVisiblePages()"
            :key="page"
            @click="goToPage(page)"
            :class="[currentPage === totalPages]"
          >
            {{ page }}
          </button>
        </div>

        <button
          @click="nextPage"
          :disabled="currentPage === totalPages"
          class="pagination-button"
        >
          Próxima ->
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Layout da aplicação */
.app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

/* Tema toggle */
.theme-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
}

.theme-button {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 2px solid var(--border-color);
  background-color: var(--bg-card);
  color: var(--text-primary);
  font-size: 20px;
  box-shadow: var(--shadow);
}

.theme-button:hover {
  transform: scale(1.1);
  border-color: var(--text-primary);
}

/* Inputs e filtros */
input,
select {
  width: 100%;
  padding: 12px;
  margin-bottom: 15px;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  background-color: var(--bg-card);
  color: var(--text-primary);
}

input:focus,
select:focus {
  outline: none;
  border-color: #646cff;
}

.filters {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.filters select,
.filters button {
  flex: 1;
  min-width: 150px;
  padding: 12px;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  background-color: var(--bg-card);
  color: var(--text-primary);
}

.filters button {
  background-color: var(--bg-secondary);
  font-weight: 500;
}

.filters button:hover {
  background-color: var(--border-color);
  transform: translateY(-1px);
}

/* Grid de personagens */
.characters-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 25px;
  margin-top: 30px;
}

.character-card {
  background-color: var(--bg-card);
  border: 2px solid var(--border-color);
  border-radius: 12px;
  padding: 20px;
  text-align: center;
  box-shadow: var(--shadow);
  transition: all 0.3s ease;
  cursor: pointer;
}

.character-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  border-color: #646cff;
}

.character-card img {
  width: 100%;
  height: 220px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 15px;
}

.character-card h3 {
  color: var(--text-primary);
  margin-bottom: 10px;
  font-size: 1.2em;
}

.character-card p {
  color: var(--text-secondary);
  font-size: 0.9em;
}

/* Loading */
.loading {
  text-align: center;
  padding: 50px;
  color: var(--text-secondary);
  font-size: 1.2em;
}

/* Paginação */
.pagination {
  margin-top: 40px;
  text-align: center;
}

.pagination-info {
  margin-bottom: 20px;
  color: var(--text-secondary);
  font-size: 0.9em;
}

.pagination-controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.pagination-button {
  padding: 10px 20px;
  background-color: var(--bg-card);
  color: var(--text-primary);
  border: 2px solid var(--border-color);
  border-radius: 8px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.pagination-button:hover:not(:disabled) {
  background-color: var(--bg-secondary);
  transform: translateY(-1px);
}

.pagination-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-numbers {
  display: flex;
  gap: 5px;
}

.page-number {
  width: 40px;
  height: 40px;
  border: 2px solid var(--border-color);
  background-color: var(--bg-card);
  color: var(--text-primary);
  border-radius: 8px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.page-number:hover {
  background-color: var(--bg-secondary);
  transform: translateY(-1px);
}

.page-number.active {
  background-color: #646cff;
  color: white;
  border-color: #646cff;
}

/* Responsivo específico */
@media (max-width: 768px) {
  .filters {
    flex-direction: column;
  }

  .filters select,
  .filters button {
    min-width: auto;
  }

  .characters-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
  }

  .theme-toggle {
    top: 10px;
    right: 10px;
  }
}
</style>
