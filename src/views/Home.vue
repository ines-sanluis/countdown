<template>
  <div>
    <Counter
      :year="2021"
      :month="8"
      :date="10"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import Counter from '@/components/Counter.vue'

export default {
  components: {
    Counter
  }
}
</script>
