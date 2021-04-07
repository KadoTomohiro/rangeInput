<template>
  <div>
    <input v-model.number="innerStart" type="number" /> -
    <input v-model.number="innerEnd" type="number" :disabled="!last" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'RangeInput',
  model: {
    prop: 'range',
    event: 'input',
  },
  props: {
    start: {
      type: Number,
      required: true,
      validator(value: number): boolean {
        return !isNaN(value)
      },
    },
    end: { type: Number, required: true },
    last: { type: Boolean },
  },
  computed: {
    innerStart: {
      get(): number {
        return this.start
      },
      set(value: number) {
        this.$emit('update:start', value)
      },
    },
    innerEnd: {
      get(): number {
        return this.end
      },
      set(value: number) {
        this.$emit('update:end', value)
      },
    },
  },
})
</script>

<style scoped></style>
