<template>
  <footer id="footer">
    <div>
      <h4>email</h4>
      <p><a href="mailto:gkfl8809@naver.com">gkfl8809@naver.com</a></p>
    </div>
    <div>
      <h4>Kakao</h4>
      <p><a href="/">@kdong1224</a></p>
    </div>
    <div>
      <h4>social</h4>
      <ul>
        <li>
          <a href="https://www.youtube.com/channel/UCHg7CuuNR5thugcU_vUmkzA"
            >Youtube</a
          >
        </li>
        <li><a href="https://www.instagram.com/noyclah_jdk/">Instargram</a></li>
        <li><a href="https://github.com/KDONG1224">Github</a></li>
        <li>
          <a href="https://kdong1224.github.io/dothome21/index.html">Dothome</a>
        </li>
        <li>
          <a href="http://kdong.dothome.co.kr/portfolio/index6.html"
            >Portfolio</a
          >
        </li>
        <li><a href="https://kdongsreact.web.app/">React</a></li>
      </ul>
    </div>
  </footer>
</template>

<style lang="scss">
#footer {
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 1200;
  width: 100%;
  color: #fff;
  border-top: 1px solid #2e2e30;
  font-family: 'Rajdhani', 'Chosunilbo_myungjo';
  font-weight: 500;
  background: #000;
  display: flex;
  align-items: center;
  bottom: 0;

  div {
    text-align: center;
    padding: 30px;

    h4,
    p,
    ul {
      display: inline-block;
    }

    h4 {
      font-family: 'Rajdhani', 'Chosunilbo_myungjo';
      font-weight: 500;
      opacity: 0.6;
      padding: 0 10px;
    }
  }

  div:nth-child(1) {
    flex: 1 1 25%;
    border-right: 1px solid #2e2e30;
  }

  div:nth-child(2) {
    flex: 1 1 20%;
    border-right: 1px solid #2e2e30;
  }

  div:nth-child(3) {
    flex: 1 1 55%;

    li {
      display: inline-block;
      padding: 0 10px;
    }
  }
}
</style>
