<template>
  <a :href="race.url" class="race">
    <div class="race__header">
      <div>
        <p class="race__date">
          <ais-highlight
            :class="{
              race__highlight: 'ais-Highlight'
            }"
            :hit="race"
            attribute="season"
          />
          - {{ niceDate(race.date) }}
        </p>
        <h3 class="race__title">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="raceName"
          />
        </h3>
        <p class="race__circuit-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Circuit.circuitName"
          />
        </p>
        <p class="race__location">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Circuit.Location.locality"
          />,
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Circuit.Location.country"
          />
        </p>
      </div>
      <div class="race__icon">
        <img :src="layoutSrc" alt="Race layout" />
      </div>
    </div>
    <div class="race__podium">
      <div class="race__podium-step race__podium-step--second">
        <span class="race__position">2</span>
        <p class="race__driver-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.second.Driver.givenName"
          />&nbsp;<ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.second.Driver.familyName"
          />
        </p>
        <p class="race__constructor-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.second.Constructor.name"
          />
        </p>
      </div>
      <div class="race__podium-step">
        <span class="race__position">1</span>
        <p class="race__driver-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.first.Driver.givenName"
          />&nbsp;<ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.first.Driver.familyName"
          />
        </p>
        <p class="race__constructor-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.first.Constructor.name"
          />
        </p>
      </div>
      <div class="race__podium-step race__podium-step--third">
        <span class="race__position">3</span>
        <p class="race__driver-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.third.Driver.givenName"
          />&nbsp;<ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.third.Driver.familyName"
          />
        </p>
        <p class="race__constructor-name">
          <ais-highlight
            :class="{ race__highlight: 'ais-Highlight' }"
            :hit="race"
            attribute="Results.third.Constructor.name"
          />
        </p>
      </div>
    </div>
  </a>
</template>

<script>
import Australia from "@/assets/australia-layout.png";
import China from "@/assets/china-layout.png";
import Japan from "@/assets/japan-layout.png";

export default {
  name: "RaceItem",

  props: {
    race: {
      type: Object,
      default: null
    }
  },

  computed: {
    layoutSrc() {
      const layouts = [Australia, China, Japan];
      const rand = () => {
        const random = Math.floor(Math.random() * 3);
        if (random === 3) {
          return rand();
        }
        return random;
      };
      return layouts[rand()];
    }
  },

  methods: {
    niceDate(date) {
      const dateObj = new Date(date);
      const options = {
        weekday: "short",
        month: "long",
        day: "numeric"
      };
      return dateObj.toLocaleDateString("en-EN", options);
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
  padding: 16px 24px;
  text-decoration: none;
  color: inherit;

  &__header {
    display: flex;
    justify-content: space-between;
  }

  &__icon {
    width: 30%;
    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }

  &__title {
    font-family: "Formula1-Regular", sans-serif;
    margin-bottom: 0.4rem;
  }

  &__highlight {
    display: inline-block;
    font-size: inherit;
  }

  &__date {
    margin-bottom: 0.4rem;
    font-size: 0.9rem;
  }

  &__location {
    font-style: italic;
  }

  &__location,
  &__circuit-name {
    opacity: 0.8;
  }

  &__podium {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    margin-top: 1rem;
  }

  &__podium-step {
    width: 30%;
    text-align: center;

    &--second {
      margin-top: 1rem;

      .race__position {
        color: silver;
      }
    }

    &--third {
      margin-top: 2rem;

      .race__position {
        color: goldenrod;
      }
    }
  }

  &__position {
    color: gold;
    display: block;
    font-family: "Formula1-Bold", sans-serif;
    font-size: 1.5rem;
    text-align: center;
    margin-bottom: 0.1rem;
  }

  &__driver-name {
    margin-bottom: 0.1rem;
  }

  &__constructor-name {
    font-size: 0.9rem;
    font-style: italic;
  }
}
</style>
