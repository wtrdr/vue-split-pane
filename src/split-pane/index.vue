<template>
  <div :style="{ cursor, userSelect }" :class="['vue-splitter-container', className]" @mouseup="onMouseUp" @mousemove="onMouseMove">

    <div :style="{ width: percent+'%' }">
      <slot name="paneL"></slot>
    </div>

    <resizer :style="{ backgroundColor: borderColor }" @mousedown.native="onMouseDown" @click.native="onClick"></resizer>

    <div :style="{ width: 100-percent+'%'}">
      <slot name="paneR"></slot>
    </div>
  </div>
</template>

<script>
  import Resizer from './resizer.vue'

  export default {
    name: 'splitPane',
    components: { Resizer, Pane },
    props: {
      minPercent: {
        type: Number,
        default: 10
      },
      defaultPercent: {
        type: Number,
        default: 50
      },
      className: String,
      borderColor: String
    },
    computed: {
      userSelect() {
        return this.active ? 'none' : ''
      },
      cursor() {
        return this.active ? 'col-resize' : ''
      }
    },
    watch: {
      defaultPercent(newValue, _oldValue) {
        this.percent = newValue
      }
    },
    data() {
      return {
        active: false,
        hasMoved: false,
        height: null,
        percent: this.defaultPercent
      }
    },
    methods: {
      onClick() {
        if (!this.hasMoved) {
          this.percent = 50
          this.$emit('resize')
        }
      },
      onMouseDown() {
        this.active = true
        this.hasMoved = false
      },
      onMouseUp() {
        this.active = false
      },
      onMouseMove(e) {
        if (e.buttons === 0 || e.which === 0) {
          this.active = false
        }

        if (this.active) {
          let offset = 0
          let target = e.currentTarget
          while (target) {
            offset += target.offsetLeft
            target = target.offsetParent
          }

          const currentPage = e.pageX
          const targetOffset = e.currentTarget.offsetWidth
          const percent = Math.floor(((currentPage - offset) / targetOffset) * 10000) / 100

          if (percent > this.minPercent && percent < 100 - this.minPercent) {
            this.percent = percent
          }

          this.$emit('resize')
          this.hasMoved = true
        }
      }
    }
  }
</script>

<style scoped>
.vue-splitter-container {
  display: flex;
}
</style>
