<template>
  <div class="range-list">
    <template v-for="(start, index) in startArray">
      <RangeInput
        v-if="isLastItem(index)"
        :key="index"
        :start.sync="startArray[index]"
        :end.sync="maximum"
        :last="isLastItem(index)"
      ></RangeInput>
      <RangeInput
        v-else
        :key="index"
        :start.sync="startArray[index]"
        :end="endArray[index]"
      ></RangeInput>
    </template>
    <div>
      <button @click="add">+</button>
      <button @click="remove">-</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue'
import { minValue, required } from 'vuelidate/lib/validators'
import { Range } from '~/types/range'
import RangeInput from '~/components/RangeInput.vue'

type Data = {
  maximum: number
  startArray: number[]
}
export default Vue.extend({
  name: 'RangeList',
  components: {
    RangeInput,
  },
  model: {
    prop: 'ranges',
    event: 'change',
  },
  validations: {
    startArray: {
      $each: {
        required,
        min: minValue(0),
      },
    },
  },
  props: {
    ranges: { type: Array as PropType<Range[]>, default: () => [] },
  },
  data(): Data {
    return {
      maximum: 0,
      startArray: [],
    }
  },
  computed: {
    endArray(): number[] {
      const shiftedStart = [...this.startArray]
      shiftedStart.shift()
      return shiftedStart.map((value) => value - 1)
    },
  },
  watch: {
    startArray() {
      this.onChange()
    },
    maximum() {
      this.onChange()
    },
  },
  created() {
    this.ranges
      .map((range) => range.start)
      .forEach((start) => {
        this.startArray.push(start)
      })
    this.maximum = [...this.startArray].pop()
  },
  methods: {
    add() {
      this.startArray.push(this.maximum + 1)
      this.maximum += 1
    },
    remove() {
      this.startArray.pop()
    },
    getEndValue(index: number): number {
      return this.endArray[index] ?? this.maximum
    },
    isLastItem(index: number) {
      return index === this.startArray.length - 1
    },
    onChange() {
      const emitRanges = this.startArray.map((start, index) => {
        return {
          start,
          end: this.getEndValue(index),
        }
      })
      this.$emit('change', emitRanges)
    },
  },
})
</script>

<style scoped></style>
