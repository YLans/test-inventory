<template>
  <section class="inventory__section">
    <ul class="inventory__list">
      <li
        class="inventory__list__item item"
        v-for="n in 25"
        :key="n"
        @drop="dropItemHandler($event, n)"
        @dragover.prevent
        @dragenter.prevent
        @click="clickItemHandle($event, n)"
        @keydown="clickItemHandle($event, n)"
      >
        <div
          class="item__content"
          v-for="item in showItem(n)"
          :key="item.id"
          draggable="true"
          @dragstart="dragItemHandler($event, item)"
        >
          <div
            class="item__content-cover"
            :style="{background: item.colors[0]}"
          >
            <div class="item__content-cover__inner" :style="{background: item.colors[1]}"></div>
          </div>
          <span class="item__content-count">{{ item.count }}</span>
        </div>
      </li>
    </ul>
    <InventoryModal
      v-show="showModal"
      :chosenItem="{...chosenItem}"
      @cancelDelete="cancelDelete"
      @deleteItem="deleteItem"
    />
  </section>
</template>

<script>
import {
  ref, computed, onMounted, onBeforeUpdate,
} from 'vue';

import InventoryModal from '@/components/InventoryModal.vue';

export default {
  name: 'InventoryItems',
  components: {
    InventoryModal,
  },
  setup() {
    const localStorageData = computed(() => (localStorage.getItem('inventoryItems') || []));
    const items = ref([]);
    const chosenItem = ref({});
    const showModal = ref(false);

    onMounted(() => {
      // get items from local storage
      if (localStorageData.value.length > 2) {
        items.value = JSON.parse(localStorageData.value);
      } else {
        // set default arr
        items.value = [
          {
            id: 1,
            count: 4,
            colors: ['rgba(127, 170, 101, 1)', 'rgba(184, 217, 152, 0.35)'],
            position: 1,
          },
          {
            id: 2,
            count: 2,
            colors: ['rgba(170, 151, 101, 1)', 'rgba(217, 187, 152, 0.35)'],
            position: 2,
          },
          {
            id: 3,
            count: 5,
            colors: ['rgba(101, 108, 170, 1)', 'rgba(116, 129, 237, 0.35)'],
            position: 3,
          },
        ];
        // set default array to local storage
        localStorage.setItem('inventoryItems', JSON.stringify(items.value));
      }
    });

    onBeforeUpdate(() => {
      const itemsString = JSON.stringify(items.value);
      if (itemsString !== JSON.stringify(localStorageData.value)) {
        localStorage.setItem('inventoryItems', itemsString);
      }
    });

    const showItem = (cellNum) => items.value.filter((el) => (el.position === cellNum));

    const dragItemHandler = (evt, el) => {
      evt.target.classList.add('dragging');
      evt.dataTransfer.dropEffect = 'move';
      evt.dataTransfer.effectAllowed = 'move';
      evt.dataTransfer.setData('itemID', el.id);
    };

    const dropItemHandler = (evt, listItem) => {
      evt.target.classList.remove('dragging');
      const itemId = evt.dataTransfer.getData('itemID');
      const item = items.value.find((el) => el.id == itemId);
      item.position = listItem;
    };

    const clickItemHandle = (evt, itemId) => {
      document.querySelectorAll('.inventory__list__item').forEach((el) => {
        el.classList.remove('active');
      });
      const listItem = evt.target.closest('.inventory__list__item');
      // handle click only if it has item
      if (listItem.querySelector('.item__content')) {
        listItem.classList.add('active');
        chosenItem.value = items.value.find((el) => el.position == itemId);
        showModal.value = true;
      }
    };

    const cancelDelete = () => {
      showModal.value = false;
    };

    const deleteItem = (itemCount) => {
      const item = items.value.find((el) => el.id === chosenItem.value.id);
      if (item.count !== itemCount) {
        item.count -= itemCount;
      } else {
        items.value.splice(items.value.indexOf(item), 1);
      }
      showModal.value = false;
    };

    return {
      items,
      localStorageData,
      chosenItem,
      showModal,
      showItem,
      dragItemHandler,
      dropItemHandler,
      clickItemHandle,
      cancelDelete,
      deleteItem,
    };
  },
};
</script>

<style lang="scss">
#inventory-app {
  .inventory {
    &__section {
      position: relative;
    }
    &__list {
      grid-area: 1/2/2/3;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      height: 100%;
      border: 1px solid #4D4D4D;
      border-radius: 12px;
      background: #4D4D4D;
      overflow: hidden;
      &__item {
        position: relative;
        width: calc(20% - 1px);
        height: calc(20% - 1px);
        background: #262626;
        transition: .3s;
        cursor: pointer;
        &:hover, &:active, &:focus, &.active {
          background: #2F2F2F;
        }
        .item__content {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100%;
          height: 100%;
          &-cover, &-cover__inner {
            width: 48px;
            height: 48px;
          }
          &-cover {
            position: relative;
            &__inner {
              position: absolute;
              top: -6px;
              right: -6px;
              backdrop-filter: blur(6px);
            }
          }
          &-count {
            position: absolute;
            right: 0;
            bottom: 0;
            width: 16px;
            height: 16px;
            padding: 2px 4px;
            border-top: 1px solid #4D4D4D;
            border-left: 1px solid #4D4D4D;
            border-radius: 6px 0px 0px 0px;
            font-size: 10px;
            line-height: 12px;
            color: rgba(#FFFFFF, .4);
          }
          &.dragging {
            border: 1px solid #4D4D4D;
            border-radius: 12px;
            cursor: grabbing;
            .item__content-count {
              display: none;
            }
          }
        }
      }
    }
  }
}
</style>
