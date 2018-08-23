<template>
  <div>
    <div class="statuses" v-for="status in statuses" :key="status.id">
      <status v-bind="status" />
    </div>
  </div>
</template>

<script>
import Status from '~/components/Status.vue'

export default {
  props: ['statuses'],
  components: {
    Status,
  },
}
</script>

<style>
</style>
