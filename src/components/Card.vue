<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(920 - 32 * 4) / Math.sqrt(cardsContext.length) - 32}px`,
      width: `${
        (((920 - 32 * 4) / Math.sqrt(cardsContext.length) - 32) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 32 * 4) / Math.sqrt(cardsContext.length) - 32) * 3) / 4) * 3
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face__front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((920 - 32 * 4) / Math.sqrt(cardsContext.length) - 32) * 3) /
              4 /
              2
            }px ${
              (((920 - 32 * 4) / Math.sqrt(cardsContext.length) - 32) * 3) /
              4 /
              2
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face__back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/images/' +
              imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    rules: {
      type: Array,
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.rules.length >= 2) return;
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnableDisabledMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face__front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 0.5rem;
  padding: 0.5rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face__back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face__back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}

.card.disabled .card__inner {
  cursor: default;
}
</style>
