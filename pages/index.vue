<template>
  <div class="container">
    <RangeList v-model="ranges"></RangeList>
    <pre>
      {{ ranges }}
    </pre>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import RangeList from '~/components/RangeList.vue'
import { Range } from '~/types/range'

type Data = {
  ranges: Range[]
}

export default Vue.extend({
  components: {
    RangeList,
  },
  data(): Data {
    return {
      ranges: [
        { start: 0, end: 0 },
        { start: 10, end: 0 },
        { start: 20, end: 0 },
        { start: 30, end: 0 },
        { start: 40, end: 0 },
        { start: 50, end: 0 },
        { start: 60, end: 0 },
        { start: 70, end: 0 },
        { start: 80, end: 0 },
      ],
    }
  },
})
</script>

<style></style>
