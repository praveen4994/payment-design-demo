<template>
  <div class="bottom-sheet" v-show="isOpen">
    <transition name="fade" mode="out-in">
      <div class="bg" @click="closeSheet" v-if="isOpen"></div>
    </transition>
    <transition name="slide-up" mode="out-in">
      <div class="modal-content" :class="modalSize" v-if="isOpen">
        <slot />
      </div>
    </transition>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  components: {},
})
export default class BottomSheet extends Vue {
  @Prop() private modalSize!: string;
  @Prop({ default: false }) private isOpen!: boolean;

  private closeSheet() {
    this.$emit("close");
  }
}
</script>

<style lang="scss">
.bottom-sheet {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 100;
  max-width: 400px;
  margin: auto;
  right: 0;
  .bg {
    background-color: #00000096;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }
  .modal-content {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    border-radius: 20px 20px 0 0;
    z-index: 10;
    background-color: white;
    max-height: 90%;
    &.half {
      min-height: 50%;
    }
    &.full {
      height: 90%;
    }
  }
}
</style>
