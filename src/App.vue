<template>
  <div id="app">
    <div class="left-nav">
      <pos-nav></pos-nav>
    </div>
    <div class="main">
      <router-view/>
    </div>
    
  </div>
</template>

<script>
import posNav from "@/components/common/nav";
export default {
  name: "App",
  components: {
    posNav
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
}
.left-nav{
  width: 70px;
}
.main {
  flex: 1;
  background-color: #eff2f7;
  height: 100%;
  overflow:hidden;
}
</style>
