<template>
  <div class="inventory__modal modal">
    <div class="modal__img"></div>
  </div>
</template>

<script>
import skeletonUrl from '@/assets/footer-skeleton.svg';
import closeIconUrl from '@/assets/footer-close.svg';

export default {
  name: 'InventoryModal',
  setup() {
    console.log(closeIconUrl);
    return {
      skeletonUrl,
      closeIconUrl,
    };
  },
};
</script>

<style lang="scss">
#inventory-app .inventory__modal {
  position: absolute;
  top: 1px;
  right: 1px;
  bottom: 1px;
  display: flex;
  width: 47%;
  padding: 55px 15px 18px;
  border-left: 1px solid #4D4D4D;
  border-radius: 0 12px 12px 0;
  background: #262626;
}
</style>
