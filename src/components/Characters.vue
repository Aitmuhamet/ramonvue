<script>
import Offcanvas from './Offcanvas.vue';
import Pagination from './Pagination.vue';

import axios from 'axios';

export default {
  components: {
    Offcanvas,
    Pagination
  },

  data() {
    return {
      filters: {
        status: 'Выберите статус',
        name: ''
      },
      charactersData: null,
      data: null,
      charactersList: null,
      prevPage: '',
      nextPage: '',
      isMovingToOtherPage: false,
      currentPage: '',
      url: 'https://rickandmortyapi.com/api/character',
      urlCurrentEpisodesList: 'https://rickandmortyapi.com/api/episode/',
      episodesList: null,
    }
  },

  mounted() {
    this.getListCharacters();
  },


  methods: {
    changeNameFilter(val) {
      this.filters.name = val;
    },
    openNextPage() {
      this.currentPage = this.nextPage;
      this.isMovingToOtherPage = true;
      this.getListCharacters();
    },
    openPrevPage() {
      this.currentPage = this.prevPage;
      this.isMovingToOtherPage = true;
      this.getListCharacters();
    },
    changeStatusFilter(val) {
      this.filters.status = val;
    },
    async setEpisodesNames() {
      this.charactersList.forEach(characterItem => {
        this.episodesList.forEach(episodeItem => {
          if (characterItem.episode[0] == episodeItem.url) {
            characterItem.firstEpisodeName = episodeItem.name;
          }
        })
      })
    },
    async getListCharacters() {
      let currentUrl = '';
      if (this.isMovingToOtherPage) {
        currentUrl = this.currentPage;
      } else {
        currentUrl = this.url;
        if (
          this.filters.name.length > 0 ||
          (this.filters.status.length > 0
            && this.filters.status !== 'Выберите статус')
        ) {
          if (this.filters.status == 'Выберите статус') this.filters.status = '';
          currentUrl += '?' + new URLSearchParams(this.filters).toString();
        }
      }

      await this.fetchData(currentUrl);
      this.charactersData = this.data;
      this.charactersList = this.data.results;

      this.charactersList.forEach(characterItem => {
        const episodeId = characterItem.episode[0].slice(40);
        this.urlCurrentEpisodesList += + episodeId + ",";
      });
      this.getListEpisodes(this.urlCurrentEpisodesList);

      this.prevPage = this.data.info.prev;
      this.nextPage = this.data.info.next;

      this.filters.status = 'Выберите статус'
      this.isMovingToOtherPage = false;
      console.log('hey u, bro');
    },
    async getListEpisodes(url) {
      await this.fetchData(url);
      this.episodesList = this.data;
      this.setEpisodesNames();
    },
    async fetchData(url) {
      try {
        const response = await axios.get(url);
        this.data = response.data
      } catch (error) {
        console.error(`Error fetching data:`, error)
      }
    }
  },

}

</script>

<template>

  <div class="btn-wrapper d-flex justify-content-end w-100">
    <button class="btn filter-btn d-flex align-items-center" type="button" data-bs-toggle="offcanvas"
      data-bs-target="#offcanvasExample" aria-controls="offcanvasExample">
      Filters
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel"
        viewBox="0 0 16 16">
        <path
          d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2z" />
      </svg>
    </button>
  </div>

  <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasExample" aria-labelledby="offcanvasExampleLabel">
    <div class="offcanvas-header">
      <h2 class="offcanvas-title" id="offcanvasExampleLabel">Filters</h2>
      <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
    </div>
    <div class="offcanvas-body">
      <div>
        Some text as placeholder. In real life you can have the elements you
        have chosen. Like, text, images, lists, etc.
      </div>
      <select v-model="filters.status" class="form-select" aria-label="Default select example">
        <option selected>Выберите статус</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <div class="input-group mb-3">
        <input @input="changeNameFilter($event.target.value)" type="text" class="form-control"
          placeholder="Введите имя персонажа" aria-label="Recipient's username" aria-describedby="button-addon2">
        <button @click="getListCharacters()" class="btn" type="button" id="button-addon2">Применить</button>
      </div>
    </div>
  </div>

  <div class="container character-content">
    <div class="characters-list">
      <article class="character-item" v-for="character in charactersList" :key="character">
        <div class="character-item__img">
          <img :src="character.image" alt="">
        </div>
        <div class="character-item__body">
          <div class="section">
            <h2>{{ character.name }}</h2>
            <span class="status">
              <span v-if="character.status === 'Alive'" class="status__icon status__icon--active"></span>
              <span v-else-if="character.status === 'unknown'" class="status__icon status__icon--inactive"></span>
              <span v-else class="status__icon"></span>
              {{ character.status }} -
              {{ character.species }}
            </span>
          </div>
          <div class="section">
            <span class="text-gray">Last known location: </span>
            <span>{{ character.location.name }}</span>
          </div>
          <div class="section">
            <span class="text-gray">First seen in:</span>
            <!-- <span>{{ getFirstEpisodeName(character.episode[0]) }}</span> -->
            <span>{{ character.firstEpisodeName }}</span>
          </div>
        </div>
      </article>
    </div>
  </div>
  <Pagination :openPrevPage="openPrevPage" :openNextPage="openNextPage" :prevPage="prevPage" :nextPage="nextPage" />
</template>

<style scoped>
.form-select,
.input-group {
  margin: 20px 0;
}

.btn-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.filter-btn,
.filter-btn:focus-visible {
  margin: 0px 50px 50px 0;
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;
  font-weight: 700;
  border: 1px solid rgb(255, 152, 0);
  background: white;
  color: rgb(51, 51, 51);
  transition: all 0.1s ease 0s;
}

.btn {
  border: 1px solid #dee2e6;
}

.btn:focus-visible {
  border: 1px solid rgb(255, 152, 0);
}

.filter-btn:hover,
.btn:hover {
  border: 1px solid transparent;
  background: rgb(255, 152, 0);
  color: rgb(255, 255, 255);
}

.filter-btn svg {
  margin-left: 5px;
}

.character-content {
  display: flex;
  flex-direction: column;
  color: darkgrey;
}

.characters-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-column-gap: 1.5rem;
  justify-items: center;
  width: 100%;
}

.character-item {
  max-width: 600px;
  width: 100%;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.character-item__img {
  flex: 2 1 0%;
  width: 100%;
}

.character-item__img img {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-position: center center;
  object-fit: cover;
}

.character-item__body {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: rgb(255, 255, 255);
  display: flex;
  flex-direction: column;
}

.character-item__body .section {
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
  -webkit-box-pack: center;
  justify-content: center;
}

.character-item__body h2 {
  font-size: 1.5rem;
}

.status {
  display: flex;
  -webkit-box-align: center;
  align-items: center;
  text-transform: capitalize;
  font-size: 16px;
  font-weight: 500
}

.status__icon {
  height: 0.5rem;
  width: 0.5rem;
  margin-right: 0.375rem;
  background: rgb(214, 61, 46);
  border-radius: 50%;
}

.status__icon--active {
  background: rgb(85, 204, 68);
}

.status__icon--inactive {
  background: grey;
}

.text-gray {
  color: rgb(158, 158, 158);
  font-size: 16px;
  font-weight: 500;
}

@media (max-width: 1200px) {
  .characters-list {
    grid-template-columns: 100%;
  }
}

@media (max-width: 576px) {
  .character-item {
    flex-direction: column;
    height: initial;
  }

  .character-item__img img {
    height: 300px;
  }
}
</style>