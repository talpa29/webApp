<template>
  <div :key="state.stateName" v-for="state in breweries">
    <div :key="brewery.id" v-for="brewery in state.breweries">
      <brewery
        @delete-brewery="$emit('delete-brewery', [brewery.id,state.stateName] )"
        @update-brewery="applyUpdate" :brewery="brewery" :state = state />
    </div>
  </div>
</template>

<script>
import brewery from './brewery.vue'
export default {
  name: 'listOfBroweries',
  components: {
    brewery,
  },
  props: {
    breweries: Array,
  },
  emits: ['delete-brewery', 'update-brewery'],
  methods:{
    applyUpdate(brewery) {
      this.$emit('update-brewery',brewery)
    },
  },
}
</script>

