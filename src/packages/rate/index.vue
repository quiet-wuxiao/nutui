<template>
  <view class="nut-rate">
    <view
      class="nut-rate-item"
      :class="[{ 'nut-rate-active': n <= state.current }]"
      v-for="n in total"
      :key="n"
      @click="onClick($event, n)"
      :style="{
        marginRight: spacing + 'px'
      }"
    >
      <nut-icon
        :size="size + 'px'"
        :color="
          n <= state.current ? (disabled ? '#CCCCCC' : activeColor) : '#CCCCCC'
        "
        :name="n <= state.current ? checkedIcon : uncheckedIcon"
      ></nut-icon>
    </view>
  </view>
</template>
<script lang="ts">
import { toRefs, watch, reactive, inject } from 'vue';
import { createComponent } from '@/utils/create';
const { componentName, create } = createComponent('rate');

export default create({
  props: {
    total: {
      type: [String, Number],
      default: 5
    },
    value: {
      type: [String, Number],
      default: 3
    },
    size: {
      type: [String, Number],
      default: 25
    },
    activeColor: {
      type: String,
      default: '#FA200C'
    },
    uncheckedIcon: {
      type: String,
      default: 'star-n'
    },
    checkedIcon: {
      type: String,
      default: 'star-fill-n'
    },
    readOnly: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    spacing: {
      type: [String, Number],
      default: 20
    }
  },
  setup(props, { emit, slots }) {
    const state = reactive({
      current: props.value
    });

    const onClick = (e: Event, idx) => {
      e.stopPropagation();
      if (props.disabled) return;
      if (props.readOnly) {
        emit('update:value', state.current);
        emit('click', state.current);
      } else {
        if (state.current == idx) {
          state.current = 0;
        } else {
          state.current = idx;
        }
        emit('update:value', state.current);
        emit('click', state.current);
      }
    };
    watch(
      () => props.value,
      newVal => {
        state.current = newVal;
      }
    );

    return {
      state,
      onClick
    };
  }
});
</script>

<style lang="scss">
@import 'index.scss';
</style>
