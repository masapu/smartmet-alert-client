<template>
  <div v-if="grayScaleSelector" id="gray-scale-select-row" :class="theme">
    <div id="gray-scale-select-text">{{ grayScaleText }}</div>
    <div id="gray-scale-select-container">
      <div
        id="gray-scale-select"
        :class="[
          'focus-ring',
          grayScale ? 'gray-scale-selected' : 'gray-scale-unselected',
        ]"
        role="button"
        tabindex="0"
        :aria-pressed="grayScale ? 'true' : 'false'"
        :aria-label="grayScaleText"
        @mousedown="preventEvents"
        @click="toggleGrayScale"
        @keydown.enter="toggleGrayScale"
        @keydown.space="toggleGrayScale">
            {{ grayScale ? 'ON' : 'OFF' }}
        </div>
    </div>
  </div>
</template>

<script>
import i18n from '../mixins/i18n'

export default {
  name: 'GrayScaleToggle',
  mixins: [i18n],
  props: {
    language: {
      type: String,
      default: import.meta.env.VITE_LANGUAGE || 'fi',
    },
    grayScaleSelector: {
      type: Boolean,
      default: false,
    },
    theme: {
      type: String,
      default: 'light-theme',
    },
  },
  computed: {
    grayScale() {
      if (this.theme == null || this.theme.length === 0) {
        return false
      }
      const themeParts = this.theme.split('-')
      return themeParts.length > 1 && themeParts[1] === 'gray'
    },
    grayScaleText() {
      return this.t('grayScale')
    },
  },
  methods: {
    toggleGrayScale(event) {
      event.preventDefault()
      if (this.theme == null || this.theme.length === 0) {
        return
      }
      const baseTheme = this.theme.split('-')[0]
      this.$emit(
        'themeChanged',
        this.grayScale ? baseTheme : `${baseTheme}-gray`
      )
    },
    preventEvents(event) {
      event.preventDefault()
    },
  },
}
</script>

<style scoped lang="scss">
@import '../scss/constants.scss';

div#gray-scale-select-row {
  width: 100%;
  margin-top: 20px;
  padding-left: 59px;
  &.narrow-screen {
    display: none;
  }
  &:not(.narrow-screen) {
    display: table;
  }
}

div#gray-scale-select-text {
  display: table-cell;
  max-width: 141px;
  padding-right: 5px;
  vertical-align: middle;
  line-height: normal;
}

div#gray-scale-select-container {
  display: table-cell;
  width: 55px;
  height: $symbol-list-line-height;
  vertical-align: middle;
}

div#gray-scale-select {
  width: 100%;
  height: $symbol-list-select-height;
  line-height: $symbol-list-select-height;
  margin: 0;
  background-repeat: no-repeat;
  background-position: center;
  cursor: pointer;
  font-family: "Noto Sans", sans-serif;
  color: $white;
  font-size: 1rem;
  forced-color-adjust: none;
  &.gray-scale-selected {
    text-align: right;
    padding-right: 22px;
  }
  &.gray-scale-unselected {
    text-align: left;
    padding-left: 22px;
    color: #303193;
    font-weight: bold;
  }
}

.light-theme .gray-scale-selected {
  background-image: url($ui-image-path + 'toggle-selected-blue' + $image-extension);
}

.dark-theme .gray-scale-selected {
  background-image: url($ui-image-path + 'toggle-selected-blue' + $image-extension);
}

.light-gray-theme .gray-scale-selected {
  background-image: url($ui-image-path + 'toggle-selected-gray' + $image-extension);
}

.dark-gray-theme .gray-scale-selected {
  background-image: url($ui-image-path + 'toggle-selected-gray' + $image-extension);
}

.light-theme .gray-scale-unselected {
  background-image: url($ui-image-path + 'toggle-unselected-light' + $image-extension);
}

.dark-theme .gray-scale-unselected {
  background-image: url($ui-image-path + 'toggle-unselected-light' + $image-extension);
}

.light-gray-theme .gray-scale-unselected {
  background-image: url($ui-image-path + 'toggle-unselected-gray' + $image-extension);
}

.dark-gray-theme .gray-scale-unselected {
  background-image: url($ui-image-path + 'toggle-unselected-dark' + $image-extension);
}

@media (max-width: 767px) {
  div#gray-scale-select-row {
    margin-top: 0;
    &.narrow-screen {
      display: table;
      div#gray-scale-select-text {
        text-align: right;
        padding-right: 10px;
      }
    }
    &:not(.narrow-screen) {
      display: none;
    }
    div#gray-scale-select-container {
      height: 100%;
    }
  }
}
</style>
