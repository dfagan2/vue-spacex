<template>
  <div class="checkbox">
    <input type="checkbox" :name="name" :id="name" v-bind:checked="checked" v-on:change="$emit(`change${name}`, $event.target.checked)">
    <label :for="name">{{ text }}</label>
  </div>
</template>

<script>
export default {
  name: 'Checkbox',
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    name: String,
    text: String
  }
}
</script>

<style scoped lang="scss">
  .checkbox {
    display: inline-flex;
    align-items: center;
    font-size: 14px;
    color: white;
    text-transform: uppercase;
    margin-left: 25px;
    cursor: pointer;

    &:hover {
      input {
        background: rgba(255,255,255,0.2);
      }
    }

    input {
      appearance: none;
      width: 22px;
      height: 22px;
      border: 1px solid white;
      border-radius: 3px;
      margin: 0 10px 0 0;
      outline: 0;
      position: relative;
      transition: background 100ms linear;
      cursor: pointer;
      flex-shrink: 0;

      &:checked:after {
        content: '';
        position: absolute;
        height: 12px;
        width: 16px;
        background: url('../assets/images/checkmark.svg') top left no-repeat / 100% auto;
        top: 4px;
        left: 2px;
        z-index: 1;
      }
    }

    label {
      cursor: pointer;
    }
  }

  @media screen and (max-width: 560px) {
    .checkbox {
      margin-bottom: 10px;
    }
  }
</style>
