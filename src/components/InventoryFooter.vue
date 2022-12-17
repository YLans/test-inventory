<template>
  <footer class="inventory__footer">
    <img :src="skeletonUrl" alt="footer image" />
    <button class="inventory__footer__btn">
      <img :src="closeIconUrl" alt="close icon" />
    </button>
  </footer>
</template>

<script>
import skeletonUrl from '@/assets/footer-skeleton.svg';
import closeIconUrl from '@/assets/footer-close.svg';

export default {
  name: 'InventoryFooter',
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
#inventory-app .inventory__footer {
  position: relative;
  grid-area: 2/1/3/3;
  display: flex;
  height: fit-content;
  padding: 18px 68px 18px 18px;
  border: 1px solid #4D4D4D;
  border-radius: 12px;
  background: #262626;
  &__btn {
    position: absolute;
    top: 8px;
    right: 8px;
    width: 24px;
    height: 24px;
    background: transparent;
  }
}
</style>
