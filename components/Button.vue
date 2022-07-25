<template>
  <button
    @click="onClick"
    :class="`py-3 px-5 rounded-lg shadow-lg text-white ${
      purple ? 'bg-purple-700' : green ? 'bg-green-700' : 'bg-blue-400'
    }`"
  >
    <slot />
  </button>
</template>

<script>
export default {
  props: ['onClick', 'purple', 'green'],
}
</script>
