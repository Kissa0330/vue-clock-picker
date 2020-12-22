<template lang="pug">

div( class="outside-click-container" @mouseover="overModal=true" @mouseout="overModal=false")
	slot(name="slotName")

</template>

<script>
export default {
  props: {
    onOutsideClick: {
      type: Function,
      default: () => {}
    },
    focused: {
      type: Boolean,
      default: false
    },
  },
  data() {
    return {
      overModal: false
    }
  },
  methods: {
    onOutsideClickHandler: function(e) {
      if (!this.overModal && this.focused) {
        this.onOutsideClick()
      }
    }
  },
  mounted() {
    document.addEventListener('click', this.onOutsideClickHandler)
  },
  beforeDestroy() {
    document.removeEventListener('click', this.onOutsideClickHandler)
  }
}
</script>

<style lang="css">
.outside-click-container {
  width:300px;
  height : 405px;
  position: fixed;
  opacity: 1;
  z-index: 2;
  top: 335px;
  left: 0;
  right: 0;
  margin: auto;
}
</style>