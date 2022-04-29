<template>
  <div class="giveFeedback">
    <h2 class="giveFeedback__title">Залишити оцінку та відгук</h2>
    <form class="giveFeedback__form" @submit.prevent="handelSubmit">
      <div class="giveFeedback__form__item_together">
        <div class="giveFeedback__form__item">
        <label class="form__label" for="">Представтесь:</label>
        <input
          class="form__input"
          :class="{error: $v.form.fullName.$anyError}"
          type="text"
          placeholder="Вкажіть ваше прізвище та імя"
          v-model="$v.form.fullName.$model"
          maxlength="50"
          :disabled="form.incognito"
        >
        </div>
        <div class="giveFeedback__form__item column">
          <input
            class="form__input"
            type="checkbox"
            v-model="$v.form.incognito.$model"
            @input="handelIncognitoInput"
          >
          <label class="form__label" for="">Анонімно</label>
        </div>
      </div>
      <div class="giveFeedback__form__item">
        <label class="form__label" for="">Оцініть роботу лікаря:</label>
        <div
          class="form__block-stars"
          @mouseleave="handleMousleave"
        >
          <div
            class="form__star"
            v-for="(item, index) in markList"
            :key="index"
            @click="setRating(index + 1)"
            @mouseenter="handleMouseenter(index + 1)"
          >
            <Star
              class="form__img-star"
              :class="{ active: index < form.rating || index < hoverStars}"
            />
            <p class="form__name-star">{{item.mark}}</p>
          </div>
        </div>
      </div>
      <div class="giveFeedback__form__item">
        <label class="form__label" for="">Напишіть відгук:</label>
        <textarea
          class="form__input textarea"
          placeholder="Вкажіть відгук до 500 символів..."
          maxlength="500"
          v-model="$v.form.feedback.$model">
        </textarea>
      </div>
      <div class="giveFeedback__form__btn-group">
        <button
          class="form__button form__button_clean"
          :disabled="!$v.$anyDirty"
          @click="cleanForm"
        >
          Очистити
        </button>
        <button class="form__button form__button_send">Надіслати </button>
      </div>
    </form>
  </div>
</template>

<script>
import Star from '@/assets/icons/Star.vue';
import { requiredIf } from 'vuelidate/lib/validators';

const emptyForm = {
  fullName: '',
  incognito: false,
  rating: 0,
  feedback: '',
};

export default {
  name: 'Feedback',
  components: {
    Star,
  },
  validations: {
    form: {
      fullName: {
        required: requiredIf((form) => !form.incognito),
      },
      incognito: {},
      rating: {},
      feedback: {},
    },
  },
  data: () => ({
    markList: [
      {
        mark: 'Дуже погано',
      },
      {
        mark: 'Погано',
      },
      {
        mark: 'Нормально',
      },
      {
        mark: 'Добре',
      },
      {
        mark: 'Дуже добре',
      },
    ],
    form: { ...emptyForm },
    hoverStars: 0,
  }),
  methods: {
    setRating(mark) {
      this.$v.form.rating.$model = mark;
    },
    handleMouseenter(mark) {
      this.hoverStars = mark;
    },
    handleMousleave() {
      this.hoverStars = 0;
    },
    cleanForm() {
      this.form = { ...emptyForm };
      this.$v.$reset();
    },
    handelIncognitoInput(event) {
      if (event.target.checked) {
        this.form.fullName = '';
      }
    },
    handelSubmit() {
      if (!this.$v.$anyDirty || this.$v.$anyError) {
        return;
      }
      this.$emit('submit', this.form);
      this.cleanForm();
    },
  },
};
</script>

<style scoped lang="scss">
  .giveFeedback{
    border-width: 4px;
    border-style: solid;
    border-image: linear-gradient(to bottom, rgb(190, 190, 240), rgb(132, 115, 206)) 1;
    width: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background: rgb(236,244,255);
    background: linear-gradient(to bottom, rgba(236,244,255,1) 0%, rgba(218,232,252,1) 100%);
    padding: 20px 40px;
    .giveFeedback__title{
      color: #150e9e;
      font-size: 25px;
      text-transform: uppercase;
      margin: 0 0 30px;
    }
    .giveFeedback__form{
      width: 100%;
      .giveFeedback__form__item_together{
        display: flex;
        justify-content: space-between;
      }
      .giveFeedback__form__item{
        display: flex;
        align-items: flex-start;
        margin-bottom: 25px;
        .form__label{
          color: #2d31ab;
          font-size: 18px;
          min-width: 200px;
        }
        .form__input{
          height: 35px;
          width: 300px;
          border: 2px solid rgb(27, 27, 27);
          padding: 0 15px;
          outline: none;
          &.textarea{
            width: 100%;
            resize: none;
            height: 100px;
            padding: 15px;
          }
          &::placeholder{
            color: #aaabad;
            font-size: 12px;
            text-align: center;
          }
          &:focus{
            outline: unset;
            border: 2px solid #150e9e;
          }
          &.error{
            border-color: #ff0101;
          }
        }
        .form__block-stars{
          display: flex;
          align-items: center;
          .form__star{
            margin-right: 10px;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            .form__img-star{
              width: 70px;
              height: 70px;
              fill: #4e4d49;
              transition: all .2s;
              &.active{
                fill: #f8d64e;
              }
            }
            .form__name-star{
              font-size: 11px;
              line-height: 1;
              color: #2d31ab;
            }
          }
        }
        &.column{
          margin-bottom: 0;
          display: flex;
          flex-direction: column;
          align-items: center;
          .form__input{
            width: 30px;
            height: 30px;
            border: 2px solid rgb(27, 27, 27);
          }
          .form__label{
            min-width: unset;
          }
        }
      }
      .giveFeedback__form__btn-group{
        display: flex;
        justify-content: flex-end;
        .form__button{
          width: calc(200px - 10px);
          height: 50px;
          color: rgb(27, 27, 27);
          font-size: 18px;
          text-transform: uppercase;
          border: 2px solid rgb(27, 27, 27);
          transition: all .3s;
          &.form__button_clean{
            background: #fff2cc;
            margin-right: 20px;
          }
          &.form__button_send{
            background: #b0e3e6;
          }
          &:hover{
            border-color: #150e9e;
            color: #150e9e;
          }
        }
      }
    }
  }

  @media (max-width: 768px) {
    .giveFeedback{
      padding: 10px;
      .giveFeedback__title{
        font-size: 20px;
        text-align: center;
      }
      .giveFeedback__form{
        .giveFeedback__form__item_together{
          flex-direction: column;
        }
        .giveFeedback__form__item{
          flex-direction: column;
          &.column {
            flex-direction: row-reverse;
          }
          .form__label{
            min-width: unset;
            width: 100%;
            margin-bottom: 10px;
          }
          .form__input{
            width: 100%;
          }
          .form__block-stars{
            flex-wrap: wrap;
            justify-content: center;
            .form__star{
              width: 50%;
              margin-right: 0;
            }
          }
        }
        .giveFeedback__form__btn-group{
          flex-direction: column;
          .form__button{
            width: 100%;
            margin-bottom: 10px;
          }
        }
      }
    }
  }
</style>
