<template>
  <div class="container">
    <div>
      <h1>Air Seating Algorithm</h1>
      <!-- <div v-for="(structure, index) in structures" :key="stucture">
        <div v-for="row in structure[0]" :key="row">
          <div v-for="col in structure[1]" :key="col">{{ lanes[index][starting] }}</div>
        </div>
      </div>-->
      <Seats/>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import Seats from '~/components/Seats'

export default {
  components: {
    Logo,
    Seats
  },
  data() {
    return {
      starting: 0,
      structures: [[2, 3], [3, 4], [3, 2], [4, 3]],
      lanes: [
        [19, 25, 1, 21, 29, 7],
        [2, 26, 27, 3, 8, 30, 'x', 9, 13, 'x', 'x', 14],
        [4, 5, 10, 11, 15, 16],
        [6, 28, 20, 12, 'x', 22, 17, 'x', 23, 18, 'x', 24]
      ]
    }
  }
}
</script>

<style lang="postcss">
.container {
  @apply max-w-5xl py-16 mx-auto;
}

h1 {
  @apply text-3xl font-semibold;
}
</style>