<template>
  <div v-if="isLoaded && !isError" class="container">
    <h1 class="main-title">Куда пойти на обед?</h1>
    <ul class="list">
      <CafeItem
        v-for="place in places"
        v-bind:key="place.id"
        v-bind:place="place"
        @set-id="openModal"
        >{{ place.name }}</CafeItem
      >
    </ul>
    <DialogPlace
      v-model:isVisible="modalVisible"
      :placeId="currentPlaceId"
      :placesCount="this.places.length"
    />
    <div class="random">
      <RandomBtn @set-id="openModal" :placesCount="this.places.length"
        >Случайный выбор</RandomBtn
      >
    </div>
  </div>
  <LoadScreen v-if="!isLoaded" />

  <ErrorScreen v-if="isError" />
</template>

<script>
import CafeItem from './CafeItem.vue';
import DialogPlace from './DialogPlace.vue';
import RandomBtn from './RandomBtn.vue';
import LoadScreen from './LoadScreen.vue';
import ErrorScreen from './ErrorScreen.vue';

export default {
  components: {
    CafeItem,
    DialogPlace,
    RandomBtn,
    LoadScreen,
    ErrorScreen,
  },
  data() {
    return {
      currentPlaceId: null,
      modalVisible: false,
      isLoaded: false,
      places: [],
      isError: false,
    };
  },

  methods: {
    openModal(id) {
      this.currentPlaceId = id;
      this.modalVisible = true;
    },
    getData() {
      const url = 'https://bandaumnikov.ru/api/test/site/get-index';
      fetch(url)
        .then((response) => {
          response.json().then((result) => {
            this.places = result.data;
          });
        })
        .catch(() => {
          this.isError = true;
        })
        .finally(() => {
          this.isLoaded = true;
        });
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}
.list {
  display: flex;
  flex-wrap: wrap;
  margin: 0;
  padding: 0;
  list-style: none;
  margin: 0 -10px;
}

.random {
  position: fixed;
  bottom: 50px;
  right: 50px;
}

.main-title {
  color: #003059;
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 30px;
  text-align: center;
}

@media (max-width: 1200px) {
  .container {
    padding: 0 20px;
    box-sizing: border-box;
  }
}
</style>
