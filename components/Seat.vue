<template>
  <div class="seat">
    <span>
      <slot></slot>
    </span>
  </div>
</template>

<script>
export default {}
</script>


<style lang="postcss" scoped>
.seat {
  @apply block mx-auto  h-10 w-10 rounded bg-red-300 flex items-center justify-center;
}
</style>

