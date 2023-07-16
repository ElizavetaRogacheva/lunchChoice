<template>
  <div v-if="isVisible" class="modal">
    <div class="modal-content">
      <div class="modal-container">
        <span class="modal-title">Куда пойти на обед?</span>
        <button
          class="modal-close-btn"
          v-on:click="closeModal"
          aria-label="Закрыть"
        ></button>
        <div v-if="!isError" class="place">
          <img
            v-if="place.photo"
            class="place-img"
            :src="place.photo"
            :alt="place.name"
          />
          <div class="place-info">
            <p class="place-name">{{ place.name }}</p>
            <p class="place-address">{{ place.address }}</p>
            <p class="place-landmark">{{ place.landmark }}</p>
            <p class="place-cuisine">
              {{ place.cuisine ? `Тип кухни: ${place.cuisine}` : '' }}
            </p>
            <p class="place-distance">
              {{ place.distance ? `Расстояние ${place.distance} м.` : '' }}
            </p>
            <p class="place-time">
              {{ place.time ? `Идти ${place.time} мин.` : '' }}
            </p>
            <p class="place-lunch">
              {{
                place.business_lunch
                  ? `Есть бизнес ланчи! Стоимость ${place.price} ₽`
                  : ''
              }}
            </p>
          </div>
        </div>
        <ErrorScreen v-else />

        <RandomBtn
          :isDisabled="!isLoaded"
          @set-id="getPlaceInfo"
          :placesCount="placesCount"
          >Хочу другое</RandomBtn
        >
      </div>
      <!-- <LoadScreen v-if="!isLoaded" /> -->
    </div>
  </div>
</template>

<script>
import RandomBtn from './RandomBtn.vue';
// import LoadScreen from './LoadScreen.vue';
import ErrorScreen from './ErrorScreen.vue';

export default {
  data() {
    return {
      isLoaded: false,
      place: Object,
      isError: false,
    };
  },
  components: {
    RandomBtn,
    // LoadScreen,
    ErrorScreen,
  },
  props: {
    isVisible: {
      type: Boolean,
      default: false,
    },
    placeId: Number,
    placesCount: Number,
  },
  methods: {
    closeModal() {
      this.$emit('update:isVisible', false);
    },
    getPlaceInfo(id) {
      this.isLoaded = false;
      const url = `https://bandaumnikov.ru/api/test/site/get-view?id=${id}`;
      fetch(url)
        .then((response) => {
          response.json().then((result) => {
            this.place = result.data;
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
  watch: {
    placeId(newValue) {
      this.getPlaceInfo(newValue);
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  width: 80%;
  max-width: 1200px;
  height: 80%;
  background-color: #fff;
  padding: 20px;
  box-sizing: border-box;
  border-radius: 15px;
  position: relative;
}

.modal-close-btn {
  position: absolute;
  z-index: 1;
  top: 20px;
  right: 20px;
  width: 30px;
  height: 30px;
  border: none;
  cursor: pointer;
  background-color: transparent;
  padding: 0;
}

.modal-close-btn::before,
.modal-close-btn::after {
  content: '';
  display: block;
  width: 2px;
  height: 30px;
  background-color: #003059;
  top: 0;
  position: absolute;
}

.modal-close-btn::after {
  left: 14px;
  transform: rotate(45deg);
}

.modal-close-btn::before {
  transform: rotate(-45deg);
  left: 14px;
}

.modal-title {
  font-size: 25px;
  font-weight: bold;
  display: block;
  margin-bottom: 20px;
  color: #003059;
}
.place {
  display: flex;
  margin-bottom: 20px;
  max-height: calc(100% - 200px);
}

.place-img {
  margin-right: 20px;
  max-height: 100%;
  object-fit: contain;
  max-width: 50%;
}
.place-info {
  text-align: left;
}

.place-info p {
  margin-bottom: 15px;
  margin-top: 0;
}
.place-name {
  font-size: 20px;
  font-weight: bold;
  color: #003059;
}
.modal-container {
  height: 100%;
}
.place-lunch {
  color: #003059;
  font-weight: bold;
}

@media (max-width: 800px) {
  .modal-content {
    width: 100%;
    height: 100%;
    max-height: 100%;
    border-radius: 0;
    overflow-y: scroll;
  }

  .modal-container {
    height: initial;
  }

  .place {
    max-height: initial;
    flex-direction: column;
  }

  .place-img {
    max-width: 100%;
    margin-bottom: 20px;
    margin-right: 0;
  }
}

.modal-content .error {
  max-height: 100px;
  min-height: 100px;
}
</style>
