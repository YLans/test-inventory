<template>
  <section class="inventory__section">
    <ul class="inventory__list">
      <li class="inventory__list__row" v-for="n in 5" :key="n">
        <div class="inventory__list__item" v-for="k in 5" :key="k">
          <span class="inventory__list__item-count"></span>
        </div>
      </li>
    </ul>
    <InventoryModal />
  </section>
</template>

<script>
import { ref, onMounted } from 'vue';

import InventoryModal from '@/components/InventoryModal.vue';

export default {
  name: 'InventoryItems',
  components: {
    InventoryModal,
  },
  setup() {
    const items = ref({});
    const item = ref({});

    onMounted(() => {
      items.value = localStorage.getItem('inventoryItems') || [
        {
          id: 1,
          count: 3,
          color: 'blue',
          position: 3,
        },
      ];
    });

    const showItem = (cellNum) => (
      items.value.length > 0 ? items.value.find((el) => (el.position === cellNum)) : {}
    );

    return {
      items,
      item,
      showItem,
    };
  },
};
</script>

<style lang="scss">
#inventory-app {
  .inventory {
    &__section {
      position: relative;
      height: fit-content;
    }
    &__list {
      grid-area: 1/2/2/3;
      display: flex;
      flex-wrap: wrap;
      height: 500px;
      border: 1px solid #4D4D4D;
      border-radius: 12px;
      background: #262626;
      overflow: hidden;
      &__row {
        display: flex;
        width: 100%;
        height: 20%;
        &:not(:last-child) {
          border-bottom: 1px solid #4D4D4D;
        }
      }
      &__item {
        width: 20%;
        height: 100%;
        &:not(:last-child) {
          border-right: 1px solid #4D4D4D;
        }
      }
    }
  }
}
</style>
