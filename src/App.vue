<template>
  <div id="app">
    <NavigationBar />
    <MainPage />
    <!-- URLに対応したコンポーネントを表示 -->
    <router-view></router-view>
  </div>
</template>

<script>
import NavigationBar from './components/NavigationBar';

export default {
  name: 'App',
  components: {
    NavigationBar,
  }
}
</script>

<style>
#app {
  height: 100vh;
}
button.transparent {
  margin: 5px;
  background: transparent;
  border: none;
}
</style>