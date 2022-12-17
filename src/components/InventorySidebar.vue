<template>
  <aside class="inventory__aside">
    <div class="inventory__aside__bg">
      <img :src="imageUrl" alt="footer image" />
    </div>
    <img :src="skeletonUrl" alt="close icon" width="190" height="198" />
  </aside>
</template>

<script>
import imageUrl from '@/assets/sidebar-img.jpg';
import skeletonUrl from '@/assets/sidebar-skeleton.svg';

export default {
  name: 'InventorySidebar',
  setup() {
    return {
      imageUrl,
      skeletonUrl,
    };
  },
};
</script>

<style lang="scss">
#inventory-app .inventory__aside {
  grid-area: 1/1/2/2;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: fit-content;
  padding: 18px 14px 24px;
  border: 1px solid #4D4D4D;
  border-radius: 12px;
  background: #262626;
  &__bg {
    position: relative;
    width: fit-content;
    margin-bottom: 20px;
    border-radius: 8px;
    overflow: hidden;
    &::after {
      content: '';
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(6px);
    }
  }
}
</style>
