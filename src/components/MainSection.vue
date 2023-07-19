<template>
  <div class="wrapper-landing">
    <div class="landing">
      <div class="search">
        <input
          type="text"
          v-model="searchQuery"
          @input="fetchData"
          placeholder="Search..."
        />
      </div>
      <div v-if="loading" class="loading">
        <Loading />
      </div>
      <div v-else>
        <div class="filters">
          <div>
            <select v-model="filter.countryCode">
              <option value="">Countries</option>
              <option
                v-for="country in uniqueCountries"
                :key="country"
                :value="country"
              >
                {{ country }}
              </option>
            </select>
            <select v-model="filter.orbitCode">
              <option value="">Orbit</option>
              <option
                v-for="orbit in uniqueOrbitRegimes"
                :key="orbit"
                :value="orbit"
              >
                {{ orbit }}
              </option>
            </select>
            <select v-model="filter.objectType">
              <option value="">Object Types</option>
              <option
                v-for="objType in uniqueObjectTypes"
                :key="objType"
                :value="objType"
              >
                {{ objType }}
              </option>
            </select>
            <button @click="clearFilters">Clear</button>
          </div>
        </div>
        <div v-if="showData" class="grid-layout">
          <SatelliteCard
            v-for="item in paginatedResults"
            :key="item.noradCatId"
            :item="item"
          />
        </div>
        <div class="not-found" v-else>
          <h1>Not Found</h1>
        </div>
        <div class="pagination">
          <button @click="previousPage" :disabled="currentPage === 1">
            <img src="/arrow_left.svg" alt="left" />
          </button>
          <span>{{ currentPage }} / {{ totalPages }}</span>
          <button @click="nextPage" :disabled="currentPage === totalPages">
            <img src="/arrow_right.svg" alt="right" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SatelliteCard from "./SatelliteCard.vue";
import Loading from "./Loading.vue";
export default {
  name: "Landing",
  components: {
    SatelliteCard,
    Loading,
  },
  data() {
    return {
      searchQuery: "",
      loading: false,
      showData: true,
      data: [],
      filter: {
        countryCode: "",
        orbitCode: "",
        objectType: "",
      },
      currentPage: 1,
      itemsPerPage: 10,
    };
  },
  created() {
    this.fetchData();
  },
  computed: {
    filteredResults() {
      let filteredData = this.data;
      if (this.filter.countryCode !== "") {
        filteredData = filteredData.filter(
          (item) => item.countryCode === this.filter.countryCode
        );
      }
      if (this.filter.orbitCode !== "") {
        filteredData = filteredData.filter(
          (item) => item.orbitCode === this.filter.orbitCode
        );
      }
      if (this.filter.objectType !== "") {
        filteredData = filteredData.filter(
          (item) => item.objectType === this.filter.objectType
        );
      }
      if (this.searchQuery.trim() === "") {
        return filteredData;
      }
      const searchQueryLower = this.searchQuery.toLowerCase().trim();
      return filteredData.filter((item) => {
        const noradCatIdMatch =
          item.noradCatId &&
          item.noradCatId.toLowerCase().includes(searchQueryLower);
        const nameMatch =
          item.name && item.name.toLowerCase().includes(searchQueryLower);
        return noradCatIdMatch || nameMatch;
      });
    },
    paginatedResults() {
      this.showData = true;
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      const content = this.filteredResults.slice(startIndex, endIndex);
      if (content.length > 0) {
        return content;
      } else {
        this.showData = false;
      }
    },
    totalPages() {
      if (this.filteredResults.length > 0) {
        this.showData = true;
      }

      return Math.ceil(this.filteredResults.length / this.itemsPerPage);
    },
    uniqueCountries() {
      const uniqueCountries = [
        ...new Set(this.data.map((item) => item.countryCode)),
      ]
        .filter((value) => value !== null)
        .sort();

      return uniqueCountries;
    },
    uniqueOrbitRegimes() {
      const uniqueOrbitRegimes = [
        ...new Set(this.data.map((item) => item.orbitCode)),
      ]
        .filter((value) => value !== null)
        .sort();

      return uniqueOrbitRegimes;
    },
    uniqueObjectTypes() {
      const uniqueObjectTypes = [
        ...new Set(this.data.map((item) => item.objectType)),
      ]
        .filter((value) => value !== null)
        .sort();

      return uniqueObjectTypes;
    },
  },
  methods: {
    async fetchData() {
      try {
        this.loading = true;
        const response = await fetch("../../satellites.json");
        this.data = await response.json();
      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    clearFilters() {
      this.showData = true;
      this.searchQuery = "";
      this.filter.countryCode = "";
      this.filter.orbitCode = "";
      this.filter.objectType = "";
    },
  },
  watch: {
    filter: {
      handler() {
        this.currentPage = 1;
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
.wrapper-landing {
  width: 50%;
  position: relative;
  min-width: 320px;
  padding: 5px;
  border-radius: 10px;
}
.landing {
  border-radius: 10px;
  background: #181818;
}
.search {
  padding: 12px;
}
.landing input {
  background: transparent;
  width: 100%;
  border: none;
  border-bottom: 2px solid white;
  padding: 10px 20px;
  color: white;
  font-size: 20px;
}
.filters {
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
  margin: 10px 0;
  width: 100%;
  gap: 100px;
  font-size: 12px;
}
.filters select {
  padding: 10px;
  text-transform: uppercase;
  background: #181818;
  border: #181818;
  color: white;
}
.grid-layout {
  height: 55vh;
  scroll-behavior: smooth;
  overflow-y: scroll;
  padding: 10px 20px;
}
.filters div {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}
.pagination {
  margin-top: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}
.pagination span {
  width: 30%;
  text-align: center;
  font-size: 18px;
  font-weight: 600;
}
button {
  color: white;
  border: none;
  background-color: transparent;
  padding: 5px 10px;
  cursor: pointer;
}
button:disabled {
  opacity: 0.2;
  cursor: not-allowed;
}
.pagination button img {
  width: 80%;
}
.loading {
  position: relative;
  width: 100%;
  height: 68vh;
}
@media only screen and (max-width: 1023px) {
  .wrapper-landing {
    width: 100%;
    margin: 1vh auto 0 auto;
    position: relative;
    max-width: 500px;
    min-width: 300px;
    padding: 2px;
    border-radius: 10px;
  }
}
@media only screen and (min-width: 1023px) {
  .wrapper-landing {
    margin: 0 auto;
    max-width: 500px;
  }
  .wrapper-landing:before,
  .wrapper-landing:after {
    content: "";
    position: absolute;
    top: 0px;
    left: 0px;
    background: linear-gradient(45deg, #403aa1, #dc446c);
    background-size: 400% 400%;
    width: 100%;
    height: 100%;
    border-radius: 10px;
    z-index: -1;
    animation: animate 3s ease alternate infinite;
  }
  @keyframes animate {
    0% {
      background-position: 0 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }
  .wrapper-landing:after {
    filter: blur(20px);
  }
}
.not-found {
  height: 55vh;
  display: grid;
  place-content: center;
}
.not-found h1 {
  margin-top: -20px;
  font-size: xx-large;
  font-weight: 800;
}
</style>
