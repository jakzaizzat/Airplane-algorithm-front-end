<template>
  <div class="container">
    <div>
      <h1>Air Seating Algorithm</h1>
      <form class="mb-4" @submit.prevent="calculate">
        <div>
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-password"
          >Number of passengers waiting in queue</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            type="number"
            v-model="form.passengers"
          >
        </div>
        <div>
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-password"
          >a 2D array that represents the rows and columns for available seats</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            type="text"
            v-model="form.array"
          >
        </div>

        <button
          class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center"
          type="submit"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            class="fill-current w-4 h-4 mr-2"
          >
            <path
              class="primary"
              d="M6 2h12a2 2 0 0 1 2 2v16a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V4c0-1.1.9-2 2-2zm2 3a1 1 0 1 0 0 2h8a1 1 0 0 0 0-2H8zm0 4a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm4 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm4 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm-8 4a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm4 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm-4 4a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm4 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2z"
            ></path>
            <rect width="2" height="6" x="15" y="13" class="secondary" rx="1"></rect>
          </svg>
          <span>Generate</span>
        </button>
      </form>
      <div v-if="lanes.length>0">
        <Seats :lanes="lanes" :structures="structures"/>
      </div>
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
      structures: {
        counter: 0,
        data: ''
      },
      lanes: [],
      form: {
        passengers: 0,
        array: ''
      }
    }
  },
  methods: {
    calculate() {
      this.structures.counter++
      this.structures.data = JSON.parse(this.form.array.replace(/'/g, '"'))
      this.$axios
        .$post(`/`, this.form)
        .then(res => {
          console.log(res)
          this.lanes = res

          this.form = {
            passengers: 0,
            array: ''
          }
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style lang="postcss">
html {
  @apply bg-white;
}
.container {
  @apply max-w-5xl py-16 mx-auto;
}

h1 {
  @apply text-3xl font-semibold mb-8;
}
</style>