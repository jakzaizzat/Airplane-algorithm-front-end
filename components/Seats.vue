<template>
  <div class="seats">
    <div class="lane flex flex-wrap">
      <div
        v-for="(structure, index) in structures"
        :key="index"
        class="flex-1 flex flex-col flex-wrap mx-2"
      >
        <div v-for="(row,i) in structure[0]" :key="row" class="flex">
          <div class="bg-red-200 m-1" v-for="col in structure[1]" :key="col">
            <Seat>{{ getValue(index) }}</Seat>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Seat from '~/components/Seat'
export default {
  components: {
    Seat
  },
  data() {
    return {
      starting: 0,
      currentLane: -1,
      seats: [],
      structures: [[2, 3], [3, 4], [3, 2], [4, 3]],
      lanes: [
        [19, 25, 1, 21, 29, 7],
        [2, 26, 27, 3, 8, 30, 'x', 9, 13, 'x', 'x', 14],
        [4, 5, 10, 11, 15, 16],
        [6, 28, 20, 12, 'x', 22, 17, 'x', 23, 18, 'x', 24]
      ]
    }
  },
  methods: {
    widthClass(column) {
      //return column;
      console.log(column)
      return 'w-1/' + column
    },
    getValue(index) {
      if (this.currentLane !== index) {
        this.currentLane = index
        this.starting = 0
      }
      var value = this.lanes[index][this.starting]
      this.starting++
      return value
    }
  }
}
</script>

<style lang="postcss" scoped>
.seats {
  @apply flex flex-1;
}

.lane {
  @apply bg-blue-200 py-3;
}
</style>
