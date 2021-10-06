<template>
  <div class="filter">
    <slot name="front"></slot>
  </div>
</template>

<script>
export default {
  name: 'Filter',
  ref: 'foo',
  methods: {
    flip () { this.flipped = !this.flipped }
  },
  data () {
    return {
      flipped: false
    }
  }
}
</script>

<style type='text/css' scoped>
  .filter {
    width: 1200px;
    height: 70px;
    margin: 0 auto;
    margin-top: 70px;
    margin-bottom: 20px;
    background: #181D26;
    border-radius: 15px;
    padding: 15px 30px;

  }
</style>
