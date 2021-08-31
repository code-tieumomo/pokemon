<template>
  <div class="screen">
    <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackfaceUrl="`images/${card}.png`"
        :card="{
          index: index,
          value: card
        }"
        @onFlip="checkRule($event)" />
  </div>
</template>

<script>
import CardFlip from "./Card";

export default {
  name: "InteractScreen",
  components: {
    CardFlip
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      }
    }
  },
  data() {
    return {
      rules: []
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);
      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        this.$refs[`card-${ this.rules[0].index }`].onEnableDisabledMode();
        this.$refs[`card-${ this.rules[1].index }`].onEnableDisabledMode();

        this.rules = [];
      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        setTimeout(() => {
          this.$refs[`card-${ this.rules[0].index }`].onFlipBackCard();
          this.$refs[`card-${ this.rules[1].index }`].onFlipBackCard();
          this.rules = [];
        }, 700);
      } else {
        return false;
      }
    }
  }
};
</script>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .card__inner {
  cursor: default;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>