<template>
<div>
  <button class="event-button" @click='onClick()'>
    {{ title }}
  </button>
  </div>
</template>

<script>
export default {
  name: 'EventButton',
  props: {
    title: String,
    onClick: Function
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.event-button {
  height: 60px;
  width: 600px;
  background-color: blueviolet;
  color: white;
  margin-top: 0.4rem;
  font-size: 1.2rem;
}

</style>
