<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=PT+Mono&family=Source+Sans+Pro:wght@400;600;700&display=swap');
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-size: 16px;
  text-transform: uppercase;
}
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: 'PT Mono', monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 24rem;
  margin: 0 auto;
}
</style>
