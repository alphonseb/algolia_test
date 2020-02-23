<template>
  <a :href="race.url" class="race">
    <h3 class="race__title">
      <ais-highlight
        :class="{ race__highlight: 'ais-Highlight' }"
        :hit="race"
        attribute="raceName"
      />
    </h3>
    <h4>{{ race.season }}</h4>
  </a>
</template>

<script>
export default {
  name: "RaceItem",

  props: {
    race: {
      type: Object,
      default: null
    }
  }
};
</script>

<style lang="scss" scoped>
.race {
  display: block;
  position: relative;
  z-index: 10;
  background-color: #fff;
  border-radius: 8px;

  &__title {
    font-family: "Formula1-Regular", sans-serif;
  }

  &__highlight {
    display: block;
  }
}
</style>
