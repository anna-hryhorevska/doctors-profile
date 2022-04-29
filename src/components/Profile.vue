<template>
  <div class="profile">
    <div class="profile__block__title">
      <h1 class="profile__title">
        Профіль лікаря
      </h1>
    </div>
    <div class="container">
      <div class="profile__block__name">
        <h2 class="profile__name">
          {{ profile.fullName }}
        </h2>
        <div class="profile__block__stars">
          <Star class="profile__star" v-for="index in new Array(profile.rating)" :key="index"/>
        </div>
        <div class="profile__block__average-score">
          <p class="profile__average-score">(Cередня оцінка: {{ profile.rating }})</p>
        </div>
      </div>
      <div class="profile__general-information">
        <div class="profile__general-information_wrap">
          <img :src="profile.img" alt="doctor" class="profile__information__img">
          <ul class="profile__data-list">
            <li class="profile__data-item">
              <p class="profile__item__title">
                Місто:
              </p>
              <p class="profile__item__description">
                {{profile.city}}
              </p>
            </li>
            <li class="profile__data-item">
              <p class="profile__item__title">
                Місце роботи:
              </p>
              <p class="profile__item__description">
                {{profile.workPlace}}
              </p>
            </li>
            <li class="profile__data-item">
              <p class="profile__item__title">
                Заклад:
              </p>
              <a :href="profile.institution.href" class="profile__item__link">
                {{profile.institution.label}}
              </a>
            </li>
          </ul>
        </div>
        <div class="profile__block-reviews">
          <ul class="profile__list-reviews">
            <li v-for="(feedback, index) in visibleFeedbacks" :key="index" class="profile__reviews">
              <h3 v-if="feedback.fullName" class="profile__reviews__author">
                {{ feedback.fullName }}
              </h3>
              <h3 v-else class="profile__reviews__author">Анонімно</h3>
              <Star
                class="profile__reviews__star"
                v-for="num in new Array(feedback.rating)"
                :key="num"
              />
              <p
                v-if="feedback.feedback"
                class="profile__reviews__description"
              >
                {{ feedback.feedback }}
              </p>
            </li>
          </ul>
          <button
            v-if="amountOfVisibleFeedbacks < feedbacks.length"
            class="button__show-more"
            @click="handelShowMoreClick"
          >
            показать еще
          </button>
        </div>
      </div>
      <div class="profile__give-feedback">
        <Feedback
          @submit="addFeedback"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Feedback from '@/components/Feedback.vue';
import Star from '@/assets/icons/Star.vue';

const defaultFeedbacks = [
  {
    fullName: 'Jon Dou',
    incognito: false,
    rating: 4,
    feedback: 'Good',
  },
  {
    fullName: '',
    incognito: true,
    rating: 2,
    feedback: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Doloribus nisi aliquid voluptate reiciendis nesciunt vero repellat laborum exercitationem rerum eius!',
  },
];

const showMoreStep = 5;

export default {
  name: 'Profile',
  components: {
    Feedback,
    Star,
  },
  data: () => ({
    profile: {
      fullName: 'Тестова Оксана Василівна',
      img: 'img/doctor.jpeg',
      rating: 3,
      city: 'Львів',
      workPlace: 'Львів',
      institution: {
        href: '#',
        label: 'Відділення надання екстренної допомоги черговим лікарем у закладі стаціонарної допомоги',
      },
    },
    feedbacks: [...defaultFeedbacks],
    amountOfVisibleFeedbacks: showMoreStep,
  }),
  computed: {
    visibleFeedbacks() {
      return this.feedbacks.slice(0, this.amountOfVisibleFeedbacks);
    },
  },
  methods: {
    addFeedback(feedback) {
      this.feedbacks.push(feedback);
    },
    handelShowMoreClick() {
      this.amountOfVisibleFeedbacks += showMoreStep;
    },
  },
};
</script>

<style lang="scss">
.profile {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  .profile__block__title{
    background-color: #06567e;
    padding: 20px 0;
    .profile__title{
      font-size: 30px;
      color: #c4cace;
      text-transform: uppercase;
      text-align: center;
      font-weight: 400;
    }
  }
  .profile__block__name{
    margin-top: 20px;
    display: flex;
    align-items: center;
    .profile__name{
      color: #7dd3d5;
      font-size: 25px;
      font-weight: 400;
      text-transform: uppercase;
      margin: 0;
    }
    .profile__block__stars{
      display: flex;
      align-items: center;
      margin-left: 10px;
      .profile__star{
        width: 30px;
        height: 30px;
        fill: #f8d64e;
      }
    }
    .profile__block__average-score{
      margin-left: 10px;
      .profile__average-score{
        font-size: 20px;
        color: #000;
      }
    }
  }
  .profile__general-information{
    border: 1px solid #00000082;
    padding: 15px;
    display: flex;
    flex-direction: column;
    .profile__general-information_wrap{
      display: flex;
    }
    .profile__information__img{
      width: 219px;
      height: 249px;
      object-fit: cover;
    }
    .profile__data-list{
      list-style: none;
      .profile__data-item{
        display: flex;
        margin-bottom: 5px;
        .profile__item__title{
          font-weight: 700;
          font-size: 20px;
          color: #000;
          min-width: 200px;
          margin: 0;
        }
        .profile__item__link{
          font-weight: 400;
          font-size: 20px;
          text-decoration: none;
          transition: all .3s;
          &:hover{
            text-decoration: underline;
          }
        }
        .profile__item__description{
          font-weight: 400;
          font-size: 20px;
          color: #000;
          margin: 0;
        }
      }
    }
    .profile__block-reviews{
      margin: 30px 0 0;
      display: flex;
      align-items: center;
      flex-direction: column;
      .profile__list-reviews{
        list-style: none;
        padding: 0;
        .profile__reviews{
          border: 3px solid #7dd3d5;
          border-radius: 5px;
          padding: 10px;
          margin: 0 0 15px;
          .profile__reviews__author{
            font-size: 18px;
            color: #000;
            font-weight: 600;
            margin: 0 0 10px;
          }
          .profile__reviews__star{
            width: 15px;
            height: 15px;
            fill: #f8d64e;
          }
          .profile__reviews__description{
            margin: 10px;
            font-size: 16px;
            color: #000;
            font-style: italic;
          }
        }
      }
      .button__show-more{
        margin: 0 auto;
        height: 50px;
        color: rgb(27, 27, 27);
        font-size: 18px;
        text-transform: uppercase;
        border: 2px solid rgb(27, 27, 27);
        transition: all .3s;
        background: rgb(125, 211, 213, 0.5);
        &:hover{
          border-color: #150e9e;
          color: #150e9e;
        }
      }
    }
  }
  .profile__give-feedback{
    display: flex;
    justify-content: center;
    margin: 50px 0;
  }
}

.container {
  max-width: 1140px;
  margin: 0 auto;
}

@media (max-width: 1220px) {
  .container {
    max-width: 900px;
  }
}

@media (max-width: 992px) {
  .container {
    max-width: 720px;
  }
  // .profile{
  //   .profile__general-information{
  //     flex-direction: column;
  //     .profile__give-feedback{
  //     }
  //   }
  // }
}

@media (max-width: 768px) {
  .container {
    max-width: unset;
    padding: 0 15px;
  }
  .profile{
    .profile__block__name{
      flex-direction: column;
      .profile__name{
        text-align: center;
        margin-bottom: 10px;
      }
    }
    .profile__general-information{
      .profile__data-list{
        padding: 0;
        .profile__data-item{
          .profile__item__title{
            min-width: unset;
            margin-right: 15px;
          }
        }
      }
      .profile__general-information_wrap{
        flex-direction: column;
      }
      .profile__information__img{
        margin: 0 auto;
      }
      .profile__give-feedback{
        position: unset;
        margin-top: 30px;
      }
      .profile__block-reviews{
        .button__show-more{
          width: 100%;
        }
      }
    }
  }
}
</style>
