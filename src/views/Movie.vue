<template>
  <Header />
  <main id="main">
    <div id="movieCont">
      <form @submit.prevent="SearchMovies()">
        <div class="search-box">
          <label for="search" class="sr-only">검색하기</label>
          <div>뷰 JS</div>
          <div>영화</div>
          <div>TV</div>
          <input
            type="search"
            id="search"
            placeholder="검색어를 입력하세요!"
            v-model="search"
          />
          <button type="submit" value="search">검색</button>
        </div>
      </form>
      <div class="bg">
        <img
          :src="
            'https://image.tmdb.org/t/p/original/' +
            nowmovieDetail.backdrop_path
          "
          alt="Movie"
        />
      </div>
      <div class="movie_container">
        <div class="movie__detail__cont">
          <div class="moivePoster">
            <img
              :src="
                'https://image.tmdb.org/t/p/original/' +
                nowmovieDetail.poster_path
              "
              alt="Movie"
            />
          </div>
          <div class="moivedesc">
            <h2 class="title">{{ nowmovieDetail.title }}</h2>
            <strong class="movieExplan"
              >{{ nowmovieDetail.release_date }}•{{
                nowmovieDetail.runtime
              }}
              분•액션</strong
            >
            <div class="movieScore">
              ⭐⭐⭐☆☆ {{ nowmovieDetail.vote_average }}
            </div>
            <p class="movieStory">
              {{ nowmovieDetail.overview }}
            </p>
            <router-link
              :to="{
                name: 'MovieDetail',
                params: {
                  id: nowmovieDetail.id,
                  title: nowmovieDetail.title,
                  overview: nowmovieDetail.overview,
                  backdrop: nowmovieDetail.backdrop_path,
                  poster: nowmovieDetail.poster_path,
                  runtime: nowmovieDetail.runtime,
                  release_date: nowmovieDetail.release_date,
                  vote_average: nowmovieDetail.vote_average,
                  genres: nowmovieDetail.genres,
                  production_companies: nowmovieDetail.production_companies,
                },
              }"
              @click="clickList"
              class="detailInfo"
              >상세정보</router-link
            >
          </div>
        </div>
      </div>
      <div v-if="searchCheck == 'OK'">
        <section class="top-list">
          <div class="container">
            <h2>TOP10 콘텐츠</h2>
            <div class="swiper-container2">
              <swiper
                :slides-per-view="7"
                :space-between="100"
                @swiper="onSwiper"
                @slideChange="onSlideChange"
              >
                <swiper-slide v-for="(movie, idx) in topmovies" :key="movie.id">
                  <h1>{{ idx + 1 }}</h1>
                  <div class="poster">
                    <img
                      :src="
                        'https://image.tmdb.org/t/p/w154/' + movie.poster_path
                      "
                    />
                  </div>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </section>

        <section class="list">
          <div class="container">
            <h2>인기있는 스트리밍</h2>
            <div class="swiper-container">
              <swiper
                :slides-per-view="7"
                :space-between="20"
                @swiper="onSwiper"
                @slideChange="onSlideChange"
                @reachEnd="swiper"
                @fromEdge="swiper"
              >
                <swiper-slide v-for="movie in popmovies" :key="movie.id">
                  <div class="poster">
                    <div class="info-img">
                      <img
                        :src="
                          'https://image.tmdb.org/t/p/w154/' + movie.poster_path
                        "
                      />
                      <span class="poster_hover"
                        ><img src="img/hover.svg" alt="▶"
                      /></span>
                    </div>
                    <div class="info">
                      <a href="#"
                        ><h3 class="list_title">
                          {{ movie.title }}
                        </h3></a
                      >
                      <p class="list_date">{{ movie.release_date }}</p>
                    </div>
                  </div>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </section>

        <section class="list">
          <div class="container">
            <h2>개봉 예정 영화</h2>
            <div class="swiper-container">
              <swiper
                :slides-per-view="7"
                :space-between="20"
                @swiper="onSwiper"
                @slideChange="onSlideChange"
                @reachEnd="swiper"
                @fromEdge="swiper"
              >
                <swiper-slide v-for="movie in upmovies" :key="movie.id">
                  <div class="poster">
                    <div class="info-img">
                      <img
                        class="poster_img"
                        :src="
                          'https://image.tmdb.org/t/p/w500' + movie.poster_path
                        "
                        alt="upcomingMovie"
                      />
                      <span class="poster_hover"
                        ><img src="img/hover.svg" alt="▶"
                      /></span>
                    </div>
                    <div class="info">
                      <a href="#"
                        ><h3 class="list_title">{{ movie.title }}</h3></a
                      >
                      <p class="list_date">{{ movie.release_date }}</p>
                    </div>
                  </div>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </section>

        <section class="list">
          <div class="container">
            <h2>마블 영화</h2>
            <div class="swiper-container">
              <swiper
                :slides-per-view="7"
                :space-between="20"
                @swiper="onSwiper"
                @slideChange="onSlideChange"
                @reachEnd="swiper"
                @fromEdge="swiper"
              >
                <swiper-slide v-for="movie in marvlemovies" :key="movie.id">
                  <div class="poster">
                    <div class="info-img">
                      <img
                        class="poster_img"
                        :src="
                          'https://image.tmdb.org/t/p/w500' + movie.poster_path
                        "
                        alt="marvlemovies"
                      />
                      <span class="poster_hover"
                        ><img src="img/hover.svg" alt="▶"
                      /></span>
                    </div>
                    <div class="info">
                      <a href="#"
                        ><h3 class="list_title">{{ movie.title }}</h3></a
                      >
                      <p class="list_date">{{ movie.release_date }}</p>
                    </div>
                  </div>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </section>

        <section class="list dc">
          <div class="container">
            <h2>DC코믹스 영화</h2>
            <div class="swiper-container">
              <swiper
                :slides-per-view="7"
                :space-between="20"
                @swiper="onSwiper"
                @slideChange="onSlideChange"
                @reachEnd="swiper"
                @fromEdge="swiper"
              >
                <swiper-slide v-for="movie in dcmovies" :key="movie.id">
                  <div class="poster">
                    <div class="info-img">
                      <img
                        class="poster_img"
                        :src="
                          'https://image.tmdb.org/t/p/w500' + movie.poster_path
                        "
                        alt="dcmovies"
                      />
                      <span class="poster_hover"
                        ><img src="img/hover.svg" alt="▶"
                      /></span>
                    </div>
                    <div class="info">
                      <a href="#"
                        ><h3 class="list_title">{{ movie.title }}</h3></a
                      >
                      <p class="list_date">{{ movie.release_date }}</p>
                    </div>
                  </div>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </section>
      </div>
      <div class="search__cont" v-else>
        <div class="search">
          <div
            v-for="movie in movies"
            :key="movie.id"
            v-on:click="handleListClick"
          >
            <img
              :src="'https://image.tmdb.org/t/p/w154/' + movie.poster_path"
              :alt="movie.id"
            />
            <p class="title">{{ movie.title }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
  <Footer />
</template>

<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { ref, watch, watchEffect } from 'vue';
import 'swiper/swiper.scss';

const search = ref('');
const movies = ref([]);
const nowmovie = ref();
const nowmovieDetail = ref();
const topmovies = ref([]);
const popmovies = ref([]);
const upmovies = ref([]);
const marvlemovies = ref([]);
const dcmovies = ref([]);
const dotenv = require('dotenv');
const searchCheck = ref('OK');
dotenv.config().parsed;
const MOVIE_API = process.env.VUE_APP_MOVIE_API_KEY;
export default {
  components: {
    Header,
    Footer,
    Swiper,
    SwiperSlide,
  },
  data: function () {
    return {
      search,
      movies,
      nowmovie,
      topmovies,
      popmovies,
      upmovies,
      marvlemovies,
      dcmovies,
      nowmovieDetail,
      searchCheck,
    };
  },
  methods: {
    handleClick: function (e) {
      nowmovie.value.id = topmovies.value[e.path[0].innerText - 1].id;
      this.MovieDetail();
    },
    handleListClick: function (e) {
      nowmovie.value.id = e.path[0].attributes.alt.textContent;
      this.MovieDetail();
    },
    clickList() {
      this.$router.push({
        name: 'MovieDetail',
        params: {
          id: nowmovieDetail.value.id,
          title: nowmovieDetail.value.title,
          overview: nowmovieDetail.value.overview,
          backdrop: nowmovieDetail.value.backdrop_path,
          poster: nowmovieDetail.value.poster_path,
          runtime: nowmovieDetail.value.runtime,
          release_date: nowmovieDetail.value.release_date,
          vote_average: nowmovieDetail.value.vote_average,
          genres: JSON.stringify(nowmovieDetail.value.genres),
          production_companies: JSON.stringify(
            nowmovieDetail.value.production_companies,
          ),
        },
      });
    },
  },
  setup() {
    const SearchMovies = () => {
      if (search.value != '') {
        fetch(
          `https://api.themoviedb.org/3/search/movie?api_key=${MOVIE_API}&query=${search.value}&language=ko-KR`,
        )
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.results;
            search.value = '';
            // console.log(data.results);
            searchCheck.value = 'NO';
            // console.log(searchCheck.value);
          })
          .catch((error) => console.log('error', error));
      }
    };
    const MovieDetail = () => {
      if (nowmovie.value != '') {
        fetch(
          `https://api.themoviedb.org/3/movie/${nowmovie.value.id}?api_key=${MOVIE_API}&language=ko-KR`,
        )
          .then((response) => response.json())
          .then((data) => {
            nowmovieDetail.value = data;
            // console.log(data);
          })
          .catch((error) => console.log('error', error));
      }
    };
    watch(nowmovie, () => {
      MovieDetail();
    });
    watchEffect(() => {
      console.log('ddds');
    });
    return {
      MovieDetail,
      SearchMovies,
    };
  },
  beforeCreate() {
    const topMovies = async () => {
      await fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=${MOVIE_API}&language=ko-KR&page=1`,
      )
        .then((response) => response.json())
        .then((data) => {
          topmovies.value = data.results;
        })
        .catch((error) => console.log('error', error));
    };

    const popMovies = () => {
      fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=${MOVIE_API}&language=ko-KR&page=1`,
      )
        .then((response) => response.json())
        .then((data) => {
          popmovies.value = data.results;
          nowmovie.value = data.results[0];
        })
        .catch((error) => console.log('error', error));
    };
    const upMovies = async () => {
      await fetch(
        `https://api.themoviedb.org/3/movie/upcoming?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          upmovies.value = data.results;
        })
        .catch((error) => console.log('error', error));
    };

    const marvleMovies = async () => {
      //   MOVIE
      //     Action          28
      //     Adventure       12
      //     Animation       16
      //     Comedy          35
      //     Crime           80
      //     Documentary     99
      //     Drama           18
      //     Family          10751
      //     Fantasy         14
      //     History         36
      //     Horror          27
      //     Music           10402
      //     Mystery         9648
      //     Romance         10749
      //     Science Fiction 878
      //     TV Movie        10770
      //     Thriller        53
      //     War             10752
      //     Western         37
      await fetch(
        `https://api.themoviedb.org/3/list/1?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          marvlemovies.value = data.items;
        })
        .catch((error) => console.log('error', error));
    };

    const dcMovies = async () => {
      await fetch(
        `https://api.themoviedb.org/3/list/3?api_key=${MOVIE_API}&language=ko-KR`,
      )
        .then((response) => response.json())
        .then((data) => {
          dcmovies.value = data.items;
        })
        .catch((error) => console.log('error', error));
    };

    searchCheck.value = 'OK';
    topMovies();
    popMovies();
    upMovies();
    marvleMovies();
    dcMovies();

    return {
      topmovies,
      popmovies,
      upmovies,
      marvlemovies,
      dcmovies,
      nowmovie,
      swiperOptions: {
        slidesPerView: 6,
        spaceBetween: 0,
        loop: true,
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
#main {
  background: #000;
  width: 100%;
}
.swiper-wrapper {
  gap: 20px !important;
}
// container
.movie_container {
  width: 1400px;
  height: 900px;
  margin: 0 auto 200px;
  position: relative;
}
#movieCont {
  width: 100%;
  height: 100%;
  font-family: 'SCoreDream';

  .top-list {
    width: 100%;
    height: 405px;
    background-color: #141414;
    box-sizing: border-box;
    padding: 50px 0;

    .container {
      h2 {
        font-weight: 500;
        color: #fff;
        font-size: 40px;
      }

      .swiper-container2 {
        width: 100%;
        height: 100%;

        .swiper-wrapper {
          display: flex;

          .swiper-slide {
            display: flex;
            position: relative;
            align-items: center;

            h1 {
              font-family: 'NEXONLv2Gothic';
              font-weight: 700;
              font-size: 250px;
              color: #000;
              -webkit-text-stroke: 5px rgb(71, 71, 71);
              line-height: 1;
            }

            .poster {
              position: absolute;
              transform: translate(55%, 0%);

              img {
                width: 180px;
              }
            }
          }
        }
      }
    }
  }
}

.list {
  background-color: #000;
  padding: 50px 0;
  color: #fff;

  &:nth-child(odd) {
    background-color: #141414;
  }

  &.dc {
    padding: 50px 0 150px;
  }

  h2 {
    font-size: 40px;
    font-weight: 500;
    color: #fff;
  }

  .swiper-container {
    height: 100%;

    .swiper-wrapper {
      display: flex;

      .swiper-slide {
        display: flex;

        .poster {
          width: 200px;
          position: relative;

          .info-img {
            cursor: pointer;

            &:hover {
              .poster_img {
                filter: brightness(50%);
              }

              .poster_hover {
                opacity: 1;
              }
            }

            .poster_img {
              width: 200px;
              height: 300px;
              transition: filter 0.3s;
            }

            .poster_hover {
              position: absolute;
              top: 45%;
              left: 50%;
              transform: translate(-50%, -50%);
              opacity: 0;
              transition: opacity 0.3s;
            }
          }

          .info {
            .list_title {
              font-size: 20px;
              font-weight: 500;
              display: block;
              white-space: nowrap;
              overflow: hidden;
              text-overflow: ellipsis;
            }

            .list_date {
              font-size: 18px;
              color: rgb(132, 132, 132);
            }
          }
        }
      }
    }
  }
}

.search__cont {
  width: 1600px;
  height: 100%;
  margin: 0 auto;
  color: #fff;
  .search {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding-bottom: 200px;
    div {
      flex: 0 0 19%;
      .title {
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        font-family: 'SCoreDream';
        margin: 10px 0 50px;
      }
    }
  }
}
.search-box {
  position: absolute;
  left: 0;
  top: 80px;
  width: 100%;
  height: 80px;
  line-height: 60px;
  padding: 10px 20px;
  background: #141414;
  z-index: 1;
  text-align: center;
  color: #fff;
  div {
    display: inline-block;
    margin-right: 20px;
  }
  input {
    width: 900px;
    line-height: 40px;
    padding: 2px 2px;
    border-radius: 10px;
    background: #141414;
    text-indent: 10px;
    color: #fff;
  }
  button {
    flex: 0 0 20%;
    padding: 10px 20px;
    font-family: 'SCoreDream';
    background: #000;
    border: 1px solid #fff;
    border-radius: 50px;
    color: #fff;
    margin-left: 20px;

    &:hover {
      background: #fff;
      border: 1px solid #000;
      color: #000;
    }
  }
}
.bg {
  position: absolute;
  z-index: 0;
  width: 100%;
  height: 900px;
  filter: brightness(10%);

  .bg img {
    height: 100%;
    object-fit: cover;
  }
}
.movie__detail__cont {
  position: relative;
  height: 500px;
  width: 100%;
  .moivePoster {
    width: 350px;
    height: 400px;
    display: inline-block;
    position: absolute;
    margin-top: 200px;
  }
  .moivedesc {
    display: inline-block;
    margin-top: 200px;
    margin-left: 25px;
    position: absolute;
    top: 0;
    left: 25%;
    color: #fff;

    .title {
      font-size: 80px;
      margin-left: -10px;
    }
    .movieExplan {
      margin-top: 40px;
      font-size: 22px;
    }
    .movieScore {
      margin-top: 10px;
    }
    .movieStory {
      margin-top: 10px;
      font-size: 18px;
      line-height: 30px;
    }
    .detailInfo {
      display: block;
      border: 1px solid #fff;
      color: #fff;
      background: #000;
      padding: 10px 50px;
      position: absolute;
      top: 125%;
      left: 0;
      border-radius: 30px;
    }
    .winnernumber {
      position: absolute;
      top: 90%;
      right: -85%;
      font-size: 400px;
      font-weight: bold;
      filter: brightness(30%);
    }
  }
}
</style>
