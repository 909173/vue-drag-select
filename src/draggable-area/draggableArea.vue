<template>
  
</template>


<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  name: 'vue-drag-select',
    props: {
      selectorClass: {
        type: String,
        required: true
      }
    },
    data () {
      return {
        mouseDown: false,
        startPoint: {
          x: 0,
          y: 0
        },
        endPoint: {
          x: 0,
          y: 0
        },
        selectedItems: [] as any[]
      }
    },
    computed: {
      selectionBox (): {left: number, top: number, width:number, height: number} {
        // Only set styling when necessary
        if (!this.mouseDown || !this.startPoint || !this.endPoint) return {left: 0, top: 0, width: 0, height: 0};
        const clientRect = this.$el.getBoundingClientRect()
        const scroll = this.getScroll()
        // Calculate position and dimensions of the selection box
        const left = Math.min(this.startPoint.x, this.endPoint.x) - clientRect.left - scroll.x
        const top = Math.min(this.startPoint.y, this.endPoint.y) - clientRect.top - scroll.y
        const width = Math.abs(this.startPoint.x - this.endPoint.x)
        const height = Math.abs(this.startPoint.y - this.endPoint.y)
        // Return the styles to be applied
        return {
          left,
          top,
          width,
          height
        }
      },
      selectionBoxStyling () {
        // Only set styling when necessary
        if (!this.mouseDown || !this.startPoint || !this.endPoint) return {}
        const { left, top, width, height }: {left: number, top: number, width: number, height: number} = this.selectionBox
        // Return the styles to be applied
        return {
          left: `${left}px`,
          top: `${top}px`,
          width: `${width}px`,
          height: `${height}px`
        }
      }
    },
    watch: {
      selectedItems (val) {
        this.$emit('change', val)
      }
    },
    methods: {
      getScroll () {
        // If we're on the server, default to 0,0
        if (typeof document === 'undefined') {
          return {
            x: 0,
            y: 0
          }
        }
        return {
          x: this.$el.scrollLeft || document.body.scrollLeft || document.documentElement.scrollLeft,
          y: this.$el.scrollTop || document.body.scrollTop || document.documentElement.scrollTop
        }
      },
      onMouseDown (event: MouseEvent) {
        // Ignore right clicks
        if (event.button === 2) return
        // Register begin point
        this.mouseDown = true
        this.startPoint = {
          x: event.pageX,
          y: event.pageY
        }
        // Start listening for mouse move and up events
        window.addEventListener('mousemove', this.onMouseMove)
        window.addEventListener('mouseup', this.onMouseUp)
      },
      onMouseMove (event: MouseEvent) {
        // Update the end point position
        if (this.mouseDown) {
          this.endPoint = {
            x: event.pageX,
            y: event.pageY
          }
          const children = this.$children.length
            ? this.$children
            : this.$children
          if (children) {
            this.selectedItems = Array.from(children).filter((item) => {
              return this.isItemSelected(item.$el || item)
            })
          }
        }
      },
      onMouseUp (event: MouseEvent) {
        // Clean up event listeners
        window.removeEventListener('mousemove', this.onMouseMove)
        window.removeEventListener('mouseup', this.onMouseUp)
        // Reset state
        this.mouseDown = false
        this.startPoint = {x:0, y:0}
        this.endPoint = {x:0, y:0}
      },
      isItemSelected (el: any) {
        if (el.classList.contains(this.selectorClass)) {
          const boxA = this.selectionBox
          const boxB = {
            top: el.offsetTop,
            left: el.offsetLeft,
            width: el.clientWidth,
            height: el.clientHeight
          }
          return !!(
            boxA.left <= boxB.left + boxB.width &&
            boxA.left + boxA.width >= boxB.left &&
            boxA.top <= boxB.top + boxB.height &&
            boxA.top + boxA.height >= boxB.top
          )
        }
        return false
      }
    },
    mounted () {
      this.$children.forEach((child) => {
        child.$on('click', (event: MouseEvent) => {
          const included = this.selectedItems.find((item) => {
            return child.$el === item.$el
          })
          if (included) {
            this.selectedItems = this.selectedItems.filter((item) => {
              return child.$el !== item.$el
            })
          } else {
            this.selectedItems.push(child)
          }
        })
      })
    },
    beforeDestroy () {
      // Remove event listeners
      window.removeEventListener('mousemove', this.onMouseMove)
      window.removeEventListener('mouseup', this.onMouseUp)
      this.$children.forEach((child) => {
        child.$off('click')
      })
    }
  } 
)
</script>


<style scoped>

</style>
