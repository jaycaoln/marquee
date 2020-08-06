<template>
  <div id="app">
    <Marquee />
    <DoubleMachine />
  </div>
</template>

<script>
import Marquee from './components/Marquee'
import DoubleMachine from './components/DoubleMachine'

export default {
  name: 'App',
  components: {
    Marquee, 
    DoubleMachine,
  }
}
</script>

<style>
#app {
  position: relative;
  background: black;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 100px;
}
h1,h2,h3,h4,h5,ul,div,li,p {
  margin: 0;
  padding: 0;
}

</style>
