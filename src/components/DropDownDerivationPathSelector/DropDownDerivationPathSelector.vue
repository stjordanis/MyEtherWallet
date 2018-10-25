<template>
  <div class="drop-down-coin-selector">
    <!--<div-->
      <!--v-if="selectedPath"-->
      <!--class="form-title-container">-->
      <!--<div class="title">{{selectedPath}}</div>-->
    <!--</div>-->

    <div class="coin-selector-click-safe-zone">
      <div
        :class="dropdownOpen ? 'dropdown-open' : ''"
        class="dropdown-input-box"
        @click="openDropdownFocustToSearchInput">
        <div class="selected-network">
          {{selectedPath}}
        </div>
        <div
          class="dropdown-open-button">
          <i
            v-if="!dropdownOpen"
            class="fa fa-chevron-down"
            aria-hidden="true"/>
          <i
            v-if="dropdownOpen"
            class="fa fa-chevron-up"
            aria-hidden="true"/>
        </div>
      </div>
      <div
        :class="dropdownOpen ? 'show-dropdown' : ''"
        class="dropdown-list-box">


<!--        <div class="dropdown-button-container">
          <b-dropdown
            id="hd-derivation-path"
            :text="selectedPath"
            class="dropdown-button-2">
            <b-dropdown-item
              v-for="(val, key) in availablePaths"
              v-if="key !== 'default'"
              :class="selectedPath === val.path ? 'active' : ''"
              :key="'base' + key"
              @click="changePath(key)">
              {{ val.path }}
            </b-dropdown-item>
            <b-dropdown-divider/>
            <b-dropdown-item>
              {{ $t('accessWallet.customPaths') }}
            </b-dropdown-item>
            <b-dropdown-item
              v-for="(val, key) in customPaths"
              :class="selectedPath.dpath === val.dpath ? 'active' : ''"
              :key="key"
              @click="changePath(key)">
              {{ val.dpath }}
            </b-dropdown-item>
            <b-dropdown-item @click="showCustomPathInput">
              {{ $t('accessWallet.addCustomPath') }}
            </b-dropdown-item>
          </b-dropdown>
        </div>-->


        <ul>
          <li v-for="(val, key) in availablePaths"
               v-if="key !== 'default'"
               :class="selectedPath === val.path ? 'active' : ''"
               :key="'base' + key"
               @click="changePath(key)">
            {{ val.path }}
          </li>
          <li><div class="line"></div></li>
          <li> {{ $t('accessWallet.customPaths') }}</li>
          <li v-for="(val, key) in customPaths"
              :class="selectedPath.dpath === val.dpath ? 'active' : ''"
              :key="key"
              @click="changePath(key)">
            {{ val.dpath }}
          </li>

          <li @click="showCustomPathInput"
              class="custom-path-button">{{ $t('accessWallet.addCustomPath') }}</li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  props: {
    selectedPath:{
      type: String,
      default: ''
    },
    availablePaths: {
      type: Array,
      default: function() {
        return [];
      }
    },
    customPaths: {
      type: Object,
      default: function() {
        return {};
      }
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
      dropdownOpen: false
    };
  },
  beforeMount() {
    // Detect all clicks on the document
    document.addEventListener('click', this.clickEvent, false);
  },
  beforeDestroy() {
    document.removeEventListener('click', this.clickEvent, false);
  },
  methods: {
    changePath(key){
      this.$emit('changePath', key)
      this.dropdownOpen = !this.dropdownOpen;
    },
    showCustomPathInput(){
      this.$emit('showCustomPathInput')
    },
    openDropdownFocustToSearchInput: function() {
      // Focus user input to the seach input.
      this.dropdownOpen = !this.dropdownOpen;
    },
    clickEvent: function(event) {
      for (let count = 0; count < event.path.length; count++) {
        if (event.path[count].className === 'coin-selector-click-safe-zone') {
          return;
        }
      }
      this.dropdownOpen = false;
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'DropDownDerivationPathSelector.scss';
</style>
