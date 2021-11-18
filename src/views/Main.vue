<template>
  <div class="light">
    <Header />
    <main id="main">
      <section id="mainCont">
        <h2 className="sr-only">메인 컨텐츠 영역입니다.</h2>
        <div className="main__cont">
          <div v-for="title in titles" v-bind:key="title.text">
            {{ title.text }}
          </div>
        </div>
      </section>
    </main>
    <Footer />
  </div>
</template>

<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';
export default {
  components: {
    Header,
    Footer,
  },

  data: function () {
    return {
      titles: [
        { text: 'we provide' },
        { text: 'visual coding' },
        { text: 'solution' },
        { text: 'for you webs' },
      ],
    };
  },
};
</script>

<style lang="scss">
#mainCont {
  height: 100vh;
  background-color: #000;
  display: flex;
  justify-content: center;
  align-items: center;
}
.main__cont {
  color: #fff;
  font-size: 10vw;
  line-height: 0.85;
  letter-spacing: -0.0275em;
  transition: all 2s;
  text-transform: uppercase;
  font-family: 'saol';

  .l2 {
    margin-left: 9vw;
  }
  .l3 {
    margin-left: 18vw;
  }
}
.main__cont.tact {
  div:nth-child(2) {
    margin-left: 9vw;
  }
  div:nth-child(3) {
    margin-left: 18vw;
  }
}

.light #mainCont {
  background-color: #f0eeeb;

  .main__cont {
    color: #000;
  }
}
</style>
