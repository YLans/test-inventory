<template>
  <InventorySidebar />
  <InventoryItems />
  <InventoryFooter />
</template>

<script>
import InventorySidebar from '@/components/InventorySidebar.vue';
import InventoryItems from '@/components/InventoryItems.vue';
import InventoryFooter from './components/InventoryFooter.vue';

export default {
  name: 'App',
  components: {
    InventorySidebar,
    InventoryItems,
    InventoryFooter,
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
}
#inventory-app {
  display: grid;
  grid-template-columns: 236px 1fr;
  grid-template-rows: min-content;
  grid-gap: 24px;
  min-height: 100vh;
  padding: 32px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #FFFFFF;
  background: #1E1E1E;
  * {
    box-sizing: border-box;
  }
  ul {
    padding: 0;
    margin: 0;
    list-style: none;
  }
  button {
    border: none;
    outline: none;
    color: inherit;
  }
  img {
    max-width: 100%;
    height: fit-content;
  }
}
</style>
