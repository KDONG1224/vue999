<template>
  <Header />
  <main id="main2">
    <section id="section01">
      <img
        class="backdrop"
        :src="'https://image.tmdb.org/t/p/w1280/' + backdrop"
      />
      <div class="bg1">
        <div class="container2">
          <div class="movie__cont">
            <div class="img">
              <img :src="'https://image.tmdb.org/t/p/w500/' + poster" />
            </div>
            <div class="detail">
              <h2>{{ title }}</h2>
              <h3>{{ release_date }} • {{ runtime }} • {{ genres[0].name }}</h3>
              <h4>
                ★★★☆☆ <strong>{{ vote_average }}</strong>
              </h4>
              <p>{{ overview }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section id="section02">
      <div class="container3">
        <h2>• Series Cast</h2>
        <swiper v-bind="swiperOptions" class="cast" @swiper="setSwiperRef">
          <swiper-slide v-for="cast in castlist" :key="cast.id" class="box">
            <img
              :src="'https://image.tmdb.org/t/p/w154/' + cast.profile_path"
            />
            <div class="castDetail">
              <strong>{{ cast.name }}</strong>
              <p>character: {{ cast.character }}</p>
              <p>popularity: {{ cast.popularity }}</p>
            </div>
          </swiper-slide>
        </swiper>
        <h2>Full cast & crew</h2>
      </div>
    </section>
    <section id="section03">
      <div class="container4">
        <h2>• Production Company</h2>
        <swiper v-bind="swiperOptions" class="cast" @swiper="setSwiperRef">
          <swiper-slide
            v-for="production in productionlist"
            :key="production.id"
            class="box"
          >
            <img
              :src="'https://image.tmdb.org/t/p/w300/' + production.logo_path"
            />
            <div class="castDetail">
              <strong>{{ production.name }}</strong>
            </div>
          </swiper-slide>
        </swiper>
      </div>
    </section>
    <section id="section05">
      <div class="container5">
        <h2>• Recommendation</h2>
        <swiper
          v-bind="swiperOptions"
          class="recommendCont"
          @swiper="setSwiperRef"
        >
          <swiper-slide
            v-for="recommendation in recommendationlist.slice(0, 20)"
            :key="recommendation.id"
            class="box"
          >
            <img
              :src="
                'https://image.tmdb.org/t/p/w154/' + recommendation.poster_path
              "
            />
            <div class="moive_info">
              <div class="movie-info-title">{{ recommendation.title }}</div>
              <div class="movie-info-date">
                {{ recommendation.release_date }}
              </div>
            </div>
          </swiper-slide>
        </swiper>
      </div>
    </section>
    <section id="section04">
      <div class="container5">
        <div class="menu">
          <h2>• REVIEW</h2>
        </div>
        <div class="comments-wrap">
          <div
            class="comment-box"
            v-for="review in reviewlist.slice(0, 10)"
            :key="review.id"
          >
            <div class="author-img">
              <img
                class="user"
                :src="
                  review.author_details.avatar_path != ''
                    ? review.author_details.avatar_path.includes('http')
                      ? review.author_details.avatar_path.substr(1)
                      : 'https://image.tmdb.org/t/p/w154/' +
                        review.author_details.avatar_path
                    : '../assets/img/productionCompany.png'
                "
                alt="logo"
              />
            </div>
            <div class="commnet-info">
              <div class="name">
                작성자 : {{ review.author_details.username }}
              </div>
              <div class="comment">{{ review.content }}</div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- //movieCont -->
  </main>
</template>
<script>
import Header from '@/components/Header.vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { ref } from 'vue';
import 'swiper/swiper.scss';
const recommendationlist = ref([]);
const castlist = ref([]);
const productionlist = ref([]);
const reviewlist = ref([]);
const dotenv = require('dotenv');
dotenv.config().parsed;
const MOVIE_API = process.env.VUE_APP_MOVIE_API;

export default {
  name: 'MovieDetail',
  props: {
    id: {
      type: String,
      default: '',
    },
    title: {
      type: String,
      default: '',
    },
    overview: {
      type: String,
      default: '',
    },
    backdrop: {
      type: String,
      default: '',
    },
    poster: {
      type: String,
      default: '',
    },
    runtime: {
      type: String,
      default: '',
    },
    release_date: {
      type: String,
      default: '',
    },
    vote_average: {
      type: String,
      default: '',
    },
    genres: {
      type: Object,
      required: true,
    },
    production_companies: {
      type: Object,
      required: true,
    },
  },
  components: {
    Header,
    Swiper,
    SwiperSlide,
  },
  data: function () {
    return {
      recommendationlist,
      castlist,
      productionlist,
      reviewlist,
    };
  },
  mounted() {
    productionlist.value = JSON.parse(this.production_companies);
    console.log(productionlist.value);
  },
  beforeCreate() {
    const recommendationList = () => {
      fetch(
        `https://api.themoviedb.org/3/movie/${this.id}/recommendations?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          recommendationlist.value = data.results;
        })
        .catch((error) => console.log('error', error));
    };
    const castList = () => {
      fetch(
        `https://api.themoviedb.org/3/movie/${this.id}/credits?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          castlist.value = data.cast;
        })
        .catch((error) => console.log('error', error));
    };
    const reviewList = () => {
      fetch(
        `https://api.themoviedb.org/3/movie/${this.id}/reviews?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          reviewlist.value = data.results;
        })
        .catch((error) => console.log('error', error));
    };
    reviewList();
    recommendationList();
    castList();
    return {
      castlist,
      recommendationlist,
      swiperOptions: {
        slidesPerView: 6,
        spaceBetween: 0,
        pagination: {
          el: '.swiper-pagination',
          clickable: true,
        },
        breakpoints: {
          1024: {
            slidesPerView: 4,
            spaceBetween: 40,
          },
          768: {
            slidesPerView: 3,
            spaceBetween: 30,
          },
          640: {
            slidesPerView: 2,
            spaceBetween: 20,
          },
          320: {
            slidesPerView: 1,
            spaceBetween: 10,
          },
        },
      },
    };
  },
};
</script>

<style lang="scss">
#main2 {
  background: #000;
  margin-top: 80px;
  height: 100%;
}
#section01 {
  height: 80vh;
  width: 100%;
  background-size: cover;
  display: flex;
  margin-top: 80px;
  justify-content: center;
  align-items: center;
  font-family: 'GmarketSans';
  .backdrop {
    position: absolute;
    width: 100%;
    height: 80vh;
    object-fit: cover;
  }
  .bg1 {
    padding: 30px 0;
    background: rgba(0, 0, 0, 0.376);
    width: 100%;
    height: 100%;
    position: relative;
  }
}
.container2 {
  width: 1600px;
  height: 700px;
  margin: 0 auto;
  position: relative;
  color: #fff;
}
.container3 {
  width: 1600px;
  height: 500px;
  margin: 0 auto;
  position: relative;
  color: #fff;
}
.container4 {
  width: 1600px;
  height: 300px;
  margin: 0 auto;
  position: relative;
  color: #fff;
}
.movie__cont {
  margin-top: 100px;
  display: flex;
  .img {
    flex: 30%;

    img {
      margin-left: 160px;
      width: 350px;
      height: 100%;
      object-fit: cover;
    }
  }
  .detail {
    flex: 70%;
    color: #fff;
    padding-left: 30px;
    h2 {
      font-size: 80px;
      font-weight: 400;
    }
    h3 {
      font-size: 22px;
      font-weight: 400;
      padding-bottom: 20px;
    }
    h4 {
      color: #ffba07;
      font-size: 20px;

      strong {
        color: #fff;
      }
    }
    p {
      width: 800px;
      padding-top: 26px;
      font-size: 22px;
      font-weight: 300;
    }
  }
}

#section02 {
  width: 100%;
  // background: rgb(255, 255, 255);
  background-color: #000;
  padding-bottom: 50px;
  color: #fff;
  h2 {
    font-size: 25px;
    font-weight: 500;
    padding-top: 20px;
  }
  .cast {
    padding-top: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .box {
      width: 200px !important;
      height: 300px;
      border-radius: 5px;
      border: 1px solid rgb(90, 90, 90);
      overflow: hidden;
      img {
        width: 100% !important;
        height: 200px;
        overflow: hidden;
        background: rgb(255, 202, 202);
        object-fit: cover;
      }
      .castDetail {
        padding: 10px;
        font-size: 15px;
        strong {
          font-size: 18px;
          font-weight: 500;
        }
        p:last-child {
          color: rgb(105, 105, 105);
        }
      }
    }
  }
}

#section03 {
  width: 100%;
  background-color: #000;
  color: #fff;
  h2 {
    font-size: 25px;
    font-weight: 500;
    padding-top: 50px;
    border-top: 2px solid #eee;
  }
  .cast {
    padding-top: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .box {
      width: 300px !important;
      border-radius: 5px;
      border: 1px solid #dad9d9;
      margin-bottom: 60px;
      img {
        width: 100% !important;
        align-items: center;
        height: 100px;
        overflow: hidden;
        background: rgb(255, 202, 202);
      }
      .castDetail {
        padding: 10px;
        font-size: 15px;
        strong {
          font-size: 18px;
          font-weight: 500;
        }
      }
    }
  }
}

#section04 {
  width: 100%;
  height: 100%;
  background-color: #000;
  color: #fff;
  padding: 50px 0;

  h2 {
    padding: 50px 0;
    margin-right: 35px;
  }

  .container5 {
    width: 1600px;
    height: 100%;
    margin: 0 auto;
    border-top: 2px solid #eee;
    height: 100%;
    .comments-wrap {
      width: 100%;
      height: 100%;
      .comment-box {
        width: 90%;
        height: 150px;
        display: flex;
        gap: 20px;
        margin-bottom: 50px;
        .author-img {
          .user {
            width: 50px;
            height: 50px;
          }
        }
        .commnet-info {
          width: 100%;
          height: 100%;
          .name {
          }
          .comment {
            width: 100%;
            height: 100%;
            overflow: scroll;
            scrollbar-width: none;
            &::-webkit-scrollbar {
              display: none; /* Chrome, Safari, Opera*/
            }
          }
        }
      }
    }
  }
}

#section05 {
  margin-top: 50px;
  width: 100%;
  color: #fff;

  h2 {
    padding: 50px 0;
    margin-right: 35px;
  }

  .container5 {
    width: 1600px;
    margin: 0 auto;
    border-top: 2px solid #eee;
    .recommendCont {
      display: flex;
      height: 400px;
      gap: 20px;
      align-items: center;
      color: #fff;
      .box {
        width: 250px !important;
        height: 300px;
        border: 1px solid #eee;
        margin-bottom: 70px;
        img {
          width: 100% !important;
          height: 100%;
          object-fit: cover;
        }
        .moive_info {
          width: 100% !important;
          margin-top: 20px;
          .movie-info-title {
            color: #fff;
          }
          .movie-info-date {
            color: #fff;
          }
        }
      }
    }
  }
}
</style>
