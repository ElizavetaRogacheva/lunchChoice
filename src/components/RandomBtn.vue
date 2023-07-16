<template>
  <div class="random-container">
    <button
      :disabled="isDisabled"
      class="random-btn"
      v-on:click="setRandomPlace"
    >
      <slot></slot>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    placesCount: null,
    isDisabled: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    setRandomPlace() {
      this.$emit('setId', this.getRandomInt());
    },

    getRandomInt() {
      return Math.floor(Math.random() * this.placesCount) + 1;
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
.random-container {
  display: flex;
  justify-content: center;
}
.random-btn {
  border: none;
  background-color: #003059;
  color: #fff;
  font-size: 20px;
  padding: 20px;
  border-radius: 15px;
  cursor: pointer;
}

.random-btn:disabled {
  opacity: 0.5;
  cursor: auto;
}
</style>
