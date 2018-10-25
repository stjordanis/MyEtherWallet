<template>
  <div v-show="show"
       @click="buttonClicked"
    :class="{'full-width': options.fullWidth, 'hide-mobile-button': onBottomOfPage && isThisMobileBottomButton}"
    class="standard-button">

    <div :class="buttonClass">
      <div
        :class="[(isThisMobileBottomButton ? 'mobile-bottom-button' : ''), (options.noMinWidth ? 'no-min-width' : '')]"
        class="the-button-box ">
        <slot></slot><!--{{ options.title }}-->
        <img
          v-if="loadingIcon"
          class="loading-left"
          src="@/assets/images/icons/loading.png">

        <img
          v-if="rightArrow && options.buttonStyle == 'green'"
          class="arrow-right"
          src="@/assets/images/icons/arrow-right.svg">

        <img
          v-if="rightArrow && options.buttonStyle == 'green-border'"
          class="arrow-right"
          src="@/assets/images/icons/arrow-right.svg">

        <img
          v-if="leftArrow && options.buttonStyle == 'green'"
          class="arrow-left"
          src="@/assets/images/icons/arrow-left.svg">

        <img
          v-if="leftArrow && options.buttonStyle == 'green-border'"
          class="arrow-left"
          src="@/assets/images/icons/arrow-green-left.svg">
      </div>
    </div>

  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      default: true
    },
    disabled: {
      type: Boolean,
      default: false
    },
    buttonStyle: {
      type: String,
      default: ''
    },
    fullWidth: {
      type: Boolean,
      default: true
    },
    loadingIcon: {
      type: Boolean,
      default: false
    },
    rightArrow: {
      type: Boolean,
      default: false
    },
    leftArrow: {
      type: Boolean,
      default: false
    },
    mobileBottom: {
      type: Boolean,
      default: false
    },
    options: {
      type: Object,
      default: function() {
        return {};
      }
    }
  },
  data() {
    return {
      onBottomOfPage: false
    };
  },
  computed: {
    isThisMobileBottomButton() {
      return this.mobileBottom;
    },
    buttonClass() {
      if (this.disabled) return 'standard-button__grey';
      switch (this.buttonStyle) {
        case 'white':
          return 'standard-button__white';
        case 'white-border':
          return 'standard-button__white-border';
        case 'grey':
        case 'disabled':
          return 'standard-button__grey';
        case 'grey-border':
          return 'standard-button__grey-border';
        case 'green':
        case 'enabled':
          return 'standard-button__green';
        case 'green-border':
          return 'standard-button__green-border';
        case 'green-noclick':
        case 'noClick':
          return 'standard-button__green-noclick';
        case 'green-transparent':
          return 'standard-button__green-transparent';
        case 'blue':
          return 'standard-button__blue';
        case 'blue-border':
          return 'standard-button__blue-border';
        default:
          return 'standard-button__green';
      }
    }
  },
  beforeMount() {
    window.addEventListener('scroll', this.onPageScroll);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.onPageScroll);
  },
  methods: {
    buttonClicked() {
      if (!this.disabled) {
        this.$emit('click');
      }
    },
    onPageScroll() {
      if (
        window.innerHeight + window.pageYOffset >=
        document.body.offsetHeight
      ) {
        this.onBottomOfPage = true;
      } else {
        this.onBottomOfPage = false;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'StandardButton.scss';
</style>
