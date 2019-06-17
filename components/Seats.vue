<template>
  <div class="seats">
    <div class="lane flex flex-wrap">
      <div
        v-for="(structure, index) in structures.data"
        class="flex-1 flex flex-col flex-wrap mx-2"
        :key="structure"
      >
        <div v-for="row in structure[0]" :key="row" class="flex justify-center">
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
  props: {
    lanes: {
      required: true,
      type: Array
    },
    structures: {
      required: true,
      type: Object
    }
  },
  components: {
    Seat
  },
  data() {
    return {
      starting: -1,
      currentLane: -1
    }
  },
  methods: {
    getValue(index) {
      if (this.currentLane !== index) {
        this.currentLane = index
        this.starting = -1
      }
      this.starting++
      return this.lanes[index][this.starting]
    }
  }
}
</script>

<style lang="postcss" scoped>
.seats {
  @apply flex flex-1;
}

.lane {
  @apply bg-blue-200 w-full py-3;
}
</style>
