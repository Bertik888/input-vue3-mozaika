<template>
  <svg
      v-if="isLoading"
      :width="size"
      :height="size"
      viewBox="0 0 20 20"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      class="ui-spinner"
  >
    <path
        d="M9.99987 1C14.9704 1 19 5.02934 19 9.99987C19 14.9704 14.9704 19 9.99987 19C5.02934 19 1 14.9707 1 10.0001"
        :stroke="color"
        :stroke-width="thickness"
    />
  </svg>
</template>

<script>
export default {
  name: 'UiSpinner',
  props: {
    isLoading: {
      type: Boolean,
      default: false
    },
    color: {
      type: String,
      default: '#00B6D0'
    },
    size: {
      type: Number,
      default: 18
    },
    thickness: {
      type: Number,
      default: 2
    }
  }
}
</script>

<style lang="scss" scoped>
svg:not(:root) {
  overflow: hidden;
}

.ui-spinner {
  vertical-align: middle;
  animation: spinner 1s linear infinite;
  transform-origin: center center;
}


@keyframes spinner {
  0% {
    transform: rotate3d(0,0,1,0)
  }
  25% {
    transform: rotate3d(0,0,1,90deg)
  }
  50% {
    transform: rotate3d(0,0,1,180deg)
  }
  75% {
    transform: rotate3d(0,0,1,270deg)
  }
  to {
    transform: rotate3d(0,0,1,359deg)
  }
}
</style>
