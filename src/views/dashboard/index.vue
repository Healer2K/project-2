<template>
  <div>
    <Card/>
    <Sale></Sale>
    <Observe></Observe>
  </div>
</template>

<script>
import Card from './Card'
import Sale from './Sale'
import Observe from './Observe'
export default {
  name: '',
  components:{
    Card,Sale,Observe
  },
  mounted(){
    //派发action
    this.$store.dispatch('getData')
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
