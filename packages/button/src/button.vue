<template>
  <button
    @click="handleClick"
    :type="nativeType"
    :class="classify"
    :disabled="disabled || loading"
    :types="nativeType"
  >
    <!-- 类名设置统一为“axe-”的前缀 -->
    <i :class="['icon', icon]" v-if="icon && !loading" />
    <i class="icon axe-icon-loading" v-if="!icon && loading" />
    <span v-if="$slots.default">
      <slot></slot>
    </span>
  </button>
</template>

<script>
import { computed } from 'vue'
const typeArray = ['default', 'primary', 'success', 'info', 'warning', 'danger']
const positionArray = ['left', 'right']
export default {
  name: 'AxeButton', // 重点是name命名，用于注册组件时使用name属性，也用于使用组件时标签名带有“axe-”的前缀，如<axe-button>
  props: {
    type: {
      type: String,
      default: 'default',
      validator(type) {
        return typeArray.includes(type)
      }
    },
    nativeType: {
      type: String,
      default: 'button',
      validator(type) {
        return ['button', 'submit', 'reset'].includes(type)
      }
    },
    icon: String,
    loading: Boolean,
    position: {
      type: String,
      default: 'left',
      require: false,
      validator(type) {
        return positionArray.includes(type)
      }
    },
    disabled: Boolean
  },
  emits: ['click'],
  setup(props, ctx) {
    const handleClick = evt => {
      ctx.emit('click', evt)
    }
    const classify = computed(() => [
      'axe-button',
      `axe-button-${props.type}`,
      props.icon && `axe-button-${props.position}`
    ])

    return {
      handleClick,
      classify
    }
  }
}
</script>
