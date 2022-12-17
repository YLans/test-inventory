<template>
  <div class="inventory__modal modal">
    <div
      class="modal__img"
      :style="{backgroundColor: Object.keys(chosenItem).length > 0 ? chosenItem.colors[0] : ''}"
    >
      <div
        class="modal__img-inner"
        :style="{backgroundColor: Object.keys(chosenItem).length > 0 ? chosenItem.colors[1] : ''}"
      ></div>
    </div>
    <div class="modal__descr">
      <img :src="skeletonUrl" alt="modal text">
    </div>
    <div class="modal__form">
      <div class="modal__form__container" v-if="showFormField">
        <label for="counter-val" class="modal__form__field">
          <input
            type="number"
            id="counter-val"
            placeholder="Введите количество"
            v-model="itemCount"
            :max="chosenItem.count"
          />
          <p class="modal__form__warn" v-if="errorMsg">{{ errorMsg }}</p>
        </label>
        <button
          class="modal__form__btn btn-cancel"
          @click="cancelDelete"
        >Отмена</button>
        <button
          class="modal__form__btn btn-submit"
          @click="deleteItem"
        >Подтвердить</button>
      </div>
      <button
        v-else
        class="modal__form__btn btn-show-form"
        @click="showFormField = !showFormField"
      >Удалить предмет</button>
    </div>
  </div>
</template>

<script>
import skeletonUrl from '@/assets/modal-skeleton.svg';
import closeIconUrl from '@/assets/footer-close.svg';

import { ref } from 'vue';

export default {
  name: 'InventoryModal',
  props: {
    chosenItem: Object,
  },
  setup(props, { emit }) {
    const showFormField = ref(false);
    const itemCount = ref();
    const errorMsg = ref('');

    const cancelDelete = (evt) => {
      evt.preventDefault();
      showFormField.value = false;
      emit('cancelDelete');
    };
    const deleteItem = (evt) => {
      evt.preventDefault();
      if (!itemCount.value) {
        errorMsg.value = 'Введите количество удаляемого предмета';
      } else if (itemCount.value > props.chosenItem.count) {
        errorMsg.value = `Введено слишком большое значение. Максимальное значение - ${props.chosenItem.count}`;
      } else {
        errorMsg.value = '';
        showFormField.value = false;
        emit('deleteItem', itemCount.value);
        itemCount.value = '';
      }
    };

    return {
      skeletonUrl,
      closeIconUrl,
      showFormField,
      itemCount,
      errorMsg,
      cancelDelete,
      deleteItem,
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
  flex-direction: column;
  align-items: center;
  width: 47%;
  padding: 55px 15px 18px;
  border-left: 1px solid #4D4D4D;
  border-radius: 0 12px 12px 0;
  background: rgba(38, 38, 38, 0.5);
  backdrop-filter: blur(8px);
  .modal {
    &__img, &__img-inner {
      width: 115px;
      height: 115px;
    }
    &__img {
      position: relative;
      margin-bottom: 30px;
      &-inner {
        position: absolute;
        top: -6px;
        right: -6px;
        backdrop-filter: blur(6px);
      }
    }
    &__descr, &__form {
      border-top: 1px solid #4D4D4D;
    }
    &__descr {
      width: 100%;
      padding-top: 16px;
    }
    &__form {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      padding: 20px 15px;
      background: rgba(38, 38, 38, 0.6);
      backdrop-filter: blur(8px);
      &__container {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
      }
      &__warn {
        font-size: 12px;
        text-align: left;
        color: #FA7272;
      }
      &__btn {
        border-radius: 8px;
        font-size: 14px;
        line-height: 17px;
        &.btn-show-form, &.btn-submit {
          color: #fff;
          background: #FA7272;
          &:hover, &:active, &:focus {
            background: #FF8888;
          }
        }
        &.btn-show-form {
          width: 100%;
          padding: 11px 0;
        }
        &.btn-cancel, &.btn-submit {
          padding: 8px 15px;
        }
        &.btn-cancel {
          min-width: 88px;
          margin-right: 10px;
          color: #2D2D2D;
          background: #FFFFFF;
        }
      }
      &__field {
        width: 100%;
        margin-bottom: 20px;
        input[type="number"] {
          width: 100%;
          padding: 12px;
          border: 1px solid #4D4D4D;
          border-radius: 4px;
          outline: none;
          font-size: 14px;
          line-height: 17px;
          color: #FFFFFF;
          background: #262626;
        }
      }
    }
  }
}
</style>
