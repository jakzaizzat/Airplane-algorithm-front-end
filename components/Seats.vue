<template>
  <div class="seats">
    <div class="lane flex flex-wrap">
      <div v-for="lane in lanes" class="flex-1 flex flex-col flex-wrap mx-2" :key="lane.length">
        <div v-for="(item,index) in lane" :key="index" class="flex justify-center">
          <div class="bg-red-200 m-1" v-for="(value, index) in item" :key="index">
            <Seat>{{ value }}</Seat>
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
    }
  },
  components: {
    Seat
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
