<template>
  <div><label><input type="checkbox" :checked="!anyRemainning" @change="allChecked">Check All</label></div>
</template>
<script>
export default {
  name: 'todo-check-all',
  props: {
    anyRemainning: {
      type: Boolean,
      required: true,
    }
  },
  methods: {
    allChecked() {
      eventBus.$emit('checkAllChanged', this.anyRemainning)
    }
  }
}

</script>
