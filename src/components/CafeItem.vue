<template>
  <li class="list__item">
    <div v-on:click="setCurrentId" class="cafe">
      <img
        v-if="place.photo"
        class="cafe__img"
        :src="place.photo"
        :alt="place.name"
        width="200px"
      />
      <div v-else class="cafe__no-img">
        <svg viewBox="0 0 24 24">
          <path
            d="M7.828 5l-1-1H22v15.172l-1-1v-.69l-3.116-3.117-.395.296-.714-.714.854-.64a.503.503 0 0 1 .657.046L21 16.067V5zM3 20v-.519l2.947-2.947a1.506 1.506 0 0 0 .677.163 1.403 1.403 0 0 0 .997-.415l2.916-2.916-.706-.707-2.916 2.916a.474.474 0 0 1-.678-.048.503.503 0 0 0-.704.007L3 18.067V5.828l-1-1V21h16.172l-1-1zM17 8.5A1.5 1.5 0 1 1 15.5 7 1.5 1.5 0 0 1 17 8.5zm-1 0a.5.5 0 1 0-.5.5.5.5 0 0 0 .5-.5zm5.646 13.854l.707-.707-20-20-.707.707z"
          />
          <path fill="none" d="M0 0h24v24H0z" />
        </svg>
      </div>
      <div class="cafe__info">
        <span class="cafe__name">{{ place.name }}</span>
        <p v-if="place.address" class="cafe__address">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0118 0z" />
            <circle cx="12" cy="10" r="3" />
          </svg>
          <span>{{ place.address }}</span>
        </p>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: 'CafeItem',
  props: {
    place: Object,
  },
  methods: {
    setCurrentId() {
      this.$emit('setId', this.place.id);
    },
  },
  emits: {
    setId: () => {
      return true;
    },
  },
};
</script>

<style scoped>
.list__item {
  max-width: 25%;
  padding: 0 10px;
  flex: 0 0 25%;
  box-sizing: border-box;
  margin-bottom: 20px;
}
.cafe__img {
  width: 100%;
  height: 250px;
  object-fit: cover;
}
.cafe {
  height: 100%;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.15);
  cursor: pointer;
  transition: box-shadow ease 0.5s;
}
.cafe:hover {
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.25);
  transition: box-shadow ease 0.5s;
}
.cafe__info {
  padding: 10px;
  box-sizing: border-box;
  text-align: left;
}
.cafe__name {
  font-size: 25px;
  line-height: 1;
  display: block;
  font-weight: bold;
  text-align: left;
  margin-bottom: 10px;
  color: #003059;
}
.cafe__no-img {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 250px;
  background-color: #e6e9eb;
}
.cafe__no-img svg {
  fill: #003059;
  width: 50px;
  height: 50px;
}
.cafe__address {
  display: flex;
}

.cafe__address svg {
  width: 24px;
  height: 24px;
  min-width: 24px;
  min-height: 24px;
  stroke: #69737b;
  fill: '#0F0F0F';

  margin-right: 5px;
}

@media (max-width: 1200px) {
  .list__item {
    max-width: 33.3%;
    padding: 0 10px;
    flex: 0 0 33.3%;
  }
}

@media (max-width: 800px) {
  .list__item {
    max-width: 50%;
    padding: 0 10px;
    flex: 0 0 50%;
  }
}

@media (max-width: 550px) {
  .list__item {
    max-width: 100%;
    padding: 0 10px;
    flex: 0 0 100%;
  }
}
</style>
