<template>
  <section>
    <article
      class="h-64 w-full bg-gray-200 rounded-lg p-5 relative overflow-hidden"
    >
      <slot name="article" />
    </article>

    <div :class="`w-full my-5 flex ${btnBoxClass}`">
      <slot name="buttons" />
    </div>
  </section>
</template>

<script>
export default {
  props: ['btnBoxClass'],
}
</script>
