<template>
  <div class="box">
    <button type="button" v-on:click="emitRefresh" class="refresh">
      <svg viewBox="0 0 48 48">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="../assets/images/refresh.svg#icon"></use>
      </svg>
    </button>
    <div class="filters">
      <Checkbox name="landed" text="Land Success" :checked='landed' v-on:changelanded="$emit('changeFilter', { filter: 'landed', value: $event })" />
      <Checkbox name="reused" text="Reused" :checked='reused' v-on:changereused="$emit('changeFilter', { filter: 'reused', value: $event })" />
      <Checkbox name="reddit" text="With Reddit" :checked='withReddit' v-on:changereddit="$emit('changeFilter', { filter: 'withReddit', value: $event })" />
    </div>
  </div>
</template>

<script>
import Checkbox from './Checkbox.vue'

export default {
  name: 'DataControls',
  components: {
    Checkbox
  },
  props: {
    landed: Boolean,
    reused: Boolean,
    withReddit: Boolean
  },
  methods: {
    emitRefresh () {
      this.$emit('refresh')
    }
  }
}
</script>

<style scoped lang="scss">
  .box {
    background: rgba(255,255,255,0.07);
    border-top: 1px solid rgba(255,255,255,0.66);
    border-right: 1px solid rgba(255,255,255,0.66);
    border-left: 1px solid rgba(255,255,255,0.66);
    border-radius: 8px 8px 0 0;
    padding: 22px;

    display: flex;
    justify-content: space-between;
    align-items: center
  }

  .refresh {
    appearance: none;
    display: inline-flex;
    position: relative;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    user-select: none;
    border: 2px solid white;
    border-radius: 50%;
    background: none;
    padding: 6px;
    cursor: pointer;
    transition: background 300ms linear;
    outline: 0;

    svg {
      width: auto;
      height: 19px;
      fill: white;
      transition: transform 300ms ease-out;
      transform: rotate(0deg);
    }

    &:hover {
      background: rgba(255,255,255,0.2);

      svg {
        transform: rotate(-180deg);
      }
    }
  }

  .filters {
    display: flex;
    align-items: center
  }

  @media screen and (max-width: 560px) {
    .filters {
      flex-direction: column;
      align-items: flex-start;
    }
  }
</style>
