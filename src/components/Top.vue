<template>
  <div class="top">
    <h1>{{ title }}</h1>
    <p>{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: 'Top',
  props: {
    title: String,
    text: String
  }
}
</script>
<style scoped>
.top {
    display: flex;
    height: 8rem;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
h1 {
    font-family: 'Source Sans Pro', sans-serif;
    font-weight: 700;
    max-width: 12rem;
    line-height: 1em;
    text-align: center;
    margin: 0.5em 0 0.4em;
    font-size: 2.1875em;
    color: black;
}
p {
    font-family: 'Source Sans Pro', sans-serif;
    font-weight: 600;
    color: rgba(34, 34, 34, 0.6);
    font-size: 0.75em;
    margin-top: auto;
}
</style>
