<template>
  <div class="container px-4">
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
            class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-1 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
            type="text"
            v-model="form.array"
          >
          <p class="text-gray-600 text-xs mb-4">Example of array : [ [2,3], [3,4], [3,2], [4,3] ]</p>
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

      <div v-if="lanes.length > 0">
        <div
          class="bg-orange-100 border-l-4 border-orange-500 text-orange-700 p-4 mb-4"
          role="alert"
        >
          <p>
            Total seats available -
            <span class="font-bold">{{seatReport.available}}</span>
          </p>
          <p v-if="seatReport.surplus < 0">
            Number of passengers didn't get a sit -
            <span
              class="font-bold"
            >{{ seatReport.surplus | positive }}</span>
          </p>
          <p v-else>
            Seats Available -
            <span class="font-bold">{{ seatReport.surplus }}</span>
          </p>
        </div>
        <Seats :lanes="lanes"/>
      </div>
    </div>
  </div>
</template>

<script>
import Seats from '~/components/Seats'

export default {
  components: {
    Seats
  },
  data() {
    return {
      lanes: [],
      form: {
        passengers: 0,
        array: ''
      },
      seatReport: {
        available: 0,
        surplus: 0
      }
    }
  },
  methods: {
    calculate() {
      try {
        var o = JSON.parse(this.form.array.replace(/'/g, '"'))
      } catch (e) {
        alert('invalid Array')
        console.log(e)
        return
      }

      var parseArr = JSON.parse(this.form.array.replace(/'/g, '"'))
      if (parseArr[0].constructor === Array) {
        this.$axios
          .$post(`/`, this.form)
          .then(res => {
            this.lanes = res
            this.passengersReport()
          })
          .catch(err => {
            console.log(err)
          })
      } else {
        alert('This is not 2D Array')
        return
      }
    },
    passengersReport() {
      this.seatReport = {
        available: 0,
        surplus: 0,
        left: 0
      }
      var seatArr = JSON.parse(this.form.array.replace(/'/g, '"'))
      seatArr.forEach(seat => {
        this.seatReport.available += seat[0] * seat[1]
      })

      this.seatReport.surplus = this.seatReport.available - this.form.passengers
    }
  },
  filters: {
    positive(value) {
      return abs(value)
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