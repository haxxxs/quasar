<script setup>
import signin from 'components/SignIn.vue'
import { onMounted, ref } from 'vue'
import { userStore } from 'stores/user'
import VueQrcode from '@chenfengyuan/vue-qrcode'

const user = userStore()
const enter = ref(true)
const balance = ref(0)
const userLink = ref('yat.li/user/' + user.pk)

onMounted(() => {
  const screenWidth = window.innerWidth
  let scaleFactor = 1 + (screenWidth / 500)
  scaleFactor = screenWidth < 668 ? scaleFactor / 1.2 : scaleFactor * 1.2
  const circleText = document.querySelector('.circleText')
  circleText.style.animation = 'rotateAnimation 30s linear infinite'

  const styleSheet = document.styleSheets[0]
  styleSheet.insertRule(`
      @keyframes rotateAnimation {
        from {
          transform: translate(-50%, -50%) scale(${scaleFactor}) rotate(0deg)
        }
        to {
          transform: translate(-50%, -50%) scale(${scaleFactor}) rotate(360deg)
        }
      }
    `, styleSheet.cssRules.length)
})
</script>

<template>
  <q-dialog style="opacity: 0.75" v-if="user.crypt && user.sk.length === 0" v-model="enter">
    <signin />
  </q-dialog>
  <!--https://tympanus.net/codrops/2012/01/02/fullscreen-background-image-slideshow-with-css3/-->
  <ul class="cb-slideshow">
    <li><span>Image 01</span>
      <div v-if="user.sk.length === 0">
        <h3>{{ $t('slider.w1') }}</h3>
        <p>&copy; <a
            href="https://unsplash.com/@chuttersnap?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">ChutterSnap</a>
        </p>
      </div>
    </li>
    <li><span>Image 02</span>
      <div v-if="user.sk.length === 0">
        <h3>{{ $t('slider.w2') }}</h3>
        <p>&copy; <a
            href="https://unsplash.com/@theshubhamdhage?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Shubham
            Dhage</a></p>
      </div>
    </li>
    <li><span>Image 03</span>
      <div v-if="user.sk.length === 0">
        <h3>{{ $t('slider.w3') }}</h3>
        <p>&copy; <a
            href="https://unsplash.com/ja/@clintadair?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Clint
            Adair</a></p>
      </div>
    </li>
    <li><span>Image 04</span>
      <div v-if="user.sk.length === 0">
        <h3>{{ $t('slider.w4') }}</h3>
        <p>&copy; <a
            href="https://unsplash.com/@urielsc26?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Uriel
            SC</a></p>
      </div>
    </li>
    <li><span>Image 05</span>
      <div v-if="user.sk.length === 0">
        <h3>{{ $t('slider.w5') }}</h3>
        <p>&copy; <a
            href="https://unsplash.com/@fabioha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">fabio</a>
        </p>
      </div>
    </li>
  </ul>
  <q-page v-if="!user.crypt" class="mainContent">
    <img :alt="$t('title')" src="/logo.svg" class="logo1" />
    <q-item class="logoWrapper" clickable tag="a" to="/sign">
      <div class="img">
        <img :alt="$t('title')" src="/logo.svg" class="logo" />
      </div>
      <q-item class="circleText">
        <svg width="250" height="250">
          <g transform="">
            <path id="textPath" d="M50,104m-23,23a23,23 0 1,1 196,0a23,23 0 1,1 -196,0" />
            <text>
              <textPath href="#textPath" startOffset="-4">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="75">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="150">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="225">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="300">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="375">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="450">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
              <textPath href="#textPath" startOffset="530">
                <tspan dy=".0em">Нажми!</tspan>
              </textPath>
            </text>
          </g>
        </svg>
      </q-item>

    </q-item>

    <div class="button">
      <q-item v-if="!user.crypt" clickable tag="a" to="/sign" class="signButton">
        <q-item-section avatar><q-icon name="stream" class="iconBlack" /></q-item-section>
        <q-item-section class="section">
          <q-item-label class="mainBtnText">{{ $t('menu.sign') }}</q-item-label>
          <q-item-label class="otherBthText">{{ $t('menu.signDesc') }}</q-item-label>
        </q-item-section>
      </q-item>
    </div>
  </q-page>
  <q-page v-if="user.crypt" class="flex flex-center">
    <div class="collumn text-center">
      <div v-if="user.sk.length > 0" style="width: 75vw">
        <h1 class="h-balance">{{ balance }} ѣ</h1>
        <div class="row flex-center q-gutter-sm" style="width: 100%">
          <!-- <q-btn push color="primary" size="l" to="/ask" label="Попросить" />
              <q-btn push color="primary" size="l" to="/spend" label="Потратить" /> -->
          <q-btn push color="primary" size="l" to="/earn" :label="$t('get')" class="q-mb-md q-mb-sm" />
          <q-btn push color="primary" size="l" to="/send" :label="$t('send')" class="q-mb-md q-mb-sm" />
        </div>
        <div class="row flex-center" style="width: 100%">
          <figure class="qrcode">
            <vue-qrcode :value="userLink" :options="{
    width: 250,
    color: {
      dark: '#3e007a',
      light: '#ffffff',
    },
  }">
            </vue-qrcode>
            <img class="qrcode__image" src="/icons/icon-qr.png" />
          </figure>
        </div>
        <div class="row flex-center q-gutter-sm" style="width: 100%">
          <!--  <q-btn round color="primary" icon="refresh" size="l" />
          <q-btn round color="primary" icon="content_copy" size="l" />
          <q-btn round color="primary" icon="chat_bubble" size="l" />
        -->
        </div>
      </div>
    </div>
  </q-page>
</template>
<style scoped lang="scss">
.mainContent {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  @media (orientation: portrait) {
    .logo1 {
      display: none;
    }

    .img {
      opacity: 0.75;
    }

    .logoWrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;

      .circleText {
        position: absolute;
        top: 50%;
        left: 50%;
        opacity: 0.75;
        transform: translate(-50%, -50%) scale(1.7);
      }

      #textPath {
        fill: none;
      }

      text {
        line-height: 1;
        text-transform: uppercase;
        font-size: 15px;
        fill: #160952;
        font-weight: 500;
        cursor: pointer;
      }

    }
  }

  @media (orientation: landscape) {
    .logoWrapper {
      display: none;
    }
  }

  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 100px;

    .signButton {
      width: 300px;
      background: white;
      opacity: 0.75;
      border-radius: 15px;
      transition: background 0.1s ease-in-out;

      .iconBlack {
        color: #311b92;
        transition: transform 0.8s ease-in-out;
      }

      .section {
        color: #311b92;
      }

      .mainBtnText {
        font-size: 20px;
      }

      .otherBthText {
        font-size: 15px;
      }

      .signButton:hover {
        transition: transform .8s ease-in-out;
      }

      &:hover {
        background: darken(#eaeaea, 15%);

        .iconBlack {
          transform: rotate(360deg);
        }
      }

      &:active {
        background: lighten(#eaeaea, 15%);
      }

    }
  }

  @media (orientation: portrait) {
    .button {
      display: none;
    }
  }
}

h1 {
  font-family: 'Roboto Slab', serif;
  font-size: 100px;
  font-weight: normal;
  padding: 0;
  margin: 0;
  margin-bottom: 1vh
}

.qrcode {
  display: inline-block;
  font-size: 0;
  margin-bottom: 0;
  position: relative;
}

.qrcode__image {
  height: 48px;
  left: 50%;
  top: 50%;
  overflow: hidden;
  position: absolute;
  transform: translate(-50%, -50%);
  width: 48px;
}

@import url('https://fonts.googleapis.com/css2?family=Sofia+Sans&display=swap');

.logo {
  opacity: 0.75;
}

@media (orientation: landscape) {
  .logo1 {
    height: 50vh;
    opacity: 0.75;
  }
}

@media (orientation: portrait) {
  .logo1 {
    width: 75vw;
  }
}

.h-balance {
  color: #ffffff;
}

.container {
  position: relative;
  width: 100%;
  height: 100%;
}

.cb-slideshow,
.cb-slideshow:after {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0px;
  left: 0px;
  z-index: 0;
}

.cb-slideshow:after {
  content: '';
  background: transparent url(/slide/pattern.png) repeat top left;
}

.cb-slideshow li span {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0px;
  left: 0px;
  color: transparent;
  background-size: cover;
  background-position: 50% 50%;
  background-repeat: none;
  opacity: 0;
  z-index: 0;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  -webkit-animation: imageAnimation 36s linear infinite 0s;
  -moz-animation: imageAnimation 36s linear infinite 0s;
  -o-animation: imageAnimation 36s linear infinite 0s;
  -ms-animation: imageAnimation 36s linear infinite 0s;
  animation: imageAnimation 36s linear infinite 0s;
}

.cb-slideshow li div {
  z-index: 1000;
  position: absolute;
  bottom: 100px;
  left: 0px;
  width: 100%;
  text-align: right;
  opacity: 0;
  -webkit-animation: titleAnimation 36s linear infinite 0s;
  -moz-animation: titleAnimation 36s linear infinite 0s;
  -o-animation: titleAnimation 36s linear infinite 0s;
  -ms-animation: titleAnimation 36s linear infinite 0s;
  animation: titleAnimation 36s linear infinite 0s;
}

.cb-slideshow li div h3 {
  font-family: 'Sofia Sans', 'Arial Narrow', Arial, sans-serif;
  font-size: 100px;
  padding: 0 30px;
  line-height: 50px;
  margin: 25px 0;
  color: #FFF;
}

.cb-slideshow li div p {
  font-size: 15px;
  padding: 0 30px;
  color: #FFF;
  font-style: italic;
}

.cb-slideshow li div p a {
  color: #FFF;
}

.cb-slideshow li:nth-child(1) span {
  background-image: url(/slide/01.webp)
}

.cb-slideshow li:nth-child(2) span {
  background-image: url(/slide/02.webp);
  -webkit-animation-delay: 6s;
  -moz-animation-delay: 6s;
  -o-animation-delay: 6s;
  -ms-animation-delay: 6s;
  animation-delay: 6s;
}

.cb-slideshow li:nth-child(3) span {
  background-image: url(/slide/03.webp);
  -webkit-animation-delay: 12s;
  -moz-animation-delay: 12s;
  -o-animation-delay: 12s;
  -ms-animation-delay: 12s;
  animation-delay: 12s;
}

.cb-slideshow li:nth-child(4) span {
  background-image: url(/slide/04.webp);
  -webkit-animation-delay: 18s;
  -moz-animation-delay: 18s;
  -o-animation-delay: 18s;
  -ms-animation-delay: 18s;
  animation-delay: 18s;
}

.cb-slideshow li:nth-child(5) span {
  background-image: url(/slide/05.webp);
  -webkit-animation-delay: 24s;
  -moz-animation-delay: 24s;
  -o-animation-delay: 24s;
  -ms-animation-delay: 24s;
  animation-delay: 24s;
}

.cb-slideshow li:nth-child(2) div {
  -webkit-animation-delay: 6s;
  -moz-animation-delay: 6s;
  -o-animation-delay: 6s;
  -ms-animation-delay: 6s;
  animation-delay: 6s;
}

.cb-slideshow li:nth-child(3) div {
  -webkit-animation-delay: 12s;
  -moz-animation-delay: 12s;
  -o-animation-delay: 12s;
  -ms-animation-delay: 12s;
  animation-delay: 12s;
}

.cb-slideshow li:nth-child(4) div {
  -webkit-animation-delay: 18s;
  -moz-animation-delay: 18s;
  -o-animation-delay: 18s;
  -ms-animation-delay: 18s;
  animation-delay: 18s;
}

.cb-slideshow li:nth-child(5) div {
  -webkit-animation-delay: 24s;
  -moz-animation-delay: 24s;
  -o-animation-delay: 24s;
  -ms-animation-delay: 24s;
  animation-delay: 24s;
}

/* Animation for the slideshow images */
@-webkit-keyframes imageAnimation {
  0% {
    opacity: 0;
    -webkit-animation-timing-function: ease-in;
  }

  8% {
    opacity: 1;
    -webkit-transform: scale(1.05);
    -webkit-animation-timing-function: ease-out;
  }

  17% {
    opacity: 1;
    -webkit-transform: scale(1.1) rotate(3deg);
  }

  25% {
    opacity: 0;
    -webkit-transform: scale(1.1) rotate(3deg);
  }

  100% {
    opacity: 0
  }
}

@-moz-keyframes imageAnimation {
  0% {
    opacity: 0;
    -moz-animation-timing-function: ease-in;
  }

  8% {
    opacity: 1;
    -moz-transform: scale(1.05);
    -moz-animation-timing-function: ease-out;
  }

  17% {
    opacity: 1;
    -moz-transform: scale(1.1) rotate(3deg);
  }

  25% {
    opacity: 0;
    -moz-transform: scale(1.1) rotate(3deg);
  }

  100% {
    opacity: 0
  }
}

@-o-keyframes imageAnimation {
  0% {
    opacity: 0;
    -o-animation-timing-function: ease-in;
  }

  8% {
    opacity: 1;
    -o-transform: scale(1.05);
    -o-animation-timing-function: ease-out;
  }

  17% {
    opacity: 1;
    -o-transform: scale(1.1) rotate(3deg);
  }

  25% {
    opacity: 0;
    -o-transform: scale(1.1) rotate(3deg);
  }

  100% {
    opacity: 0
  }
}

@-ms-keyframes imageAnimation {
  0% {
    opacity: 0;
    -ms-animation-timing-function: ease-in;
  }

  8% {
    opacity: 1;
    -ms-transform: scale(1.05);
    -ms-animation-timing-function: ease-out;
  }

  17% {
    opacity: 1;
    -ms-transform: scale(1.1) rotate(3deg);
  }

  25% {
    opacity: 0;
    -ms-transform: scale(1.1) rotate(3deg);
  }

  100% {
    opacity: 0
  }
}

@keyframes imageAnimation {
  0% {
    opacity: 0;
    animation-timing-function: ease-in;
  }

  8% {
    opacity: 1;
    transform: scale(1.05);
    animation-timing-function: ease-out;
  }

  17% {
    opacity: 1;
    transform: scale(1.1) rotate(3deg);
  }

  25% {
    opacity: 0;
    transform: scale(1.1) rotate(3deg);
  }

  100% {
    opacity: 0
  }
}

@-webkit-keyframes titleAnimation {
  0% {
    opacity: 0;
    -webkit-transform: translateX(200px);
  }

  8% {
    opacity: 1;
    -webkit-transform: translateX(0px);
  }

  17% {
    opacity: 1;
    -webkit-transform: translateX(0px);
  }

  19% {
    opacity: 0;
    -webkit-transform: translateX(-400px);
  }

  25% {
    opacity: 0
  }

  100% {
    opacity: 0
  }
}

@-moz-keyframes titleAnimation {
  0% {
    opacity: 0;
    -moz-transform: translateX(200px);
  }

  8% {
    opacity: 1;
    -moz-transform: translateX(0px);
  }

  17% {
    opacity: 1;
    -moz-transform: translateX(0px);
  }

  19% {
    opacity: 0;
    -moz-transform: translateX(-400px);
  }

  25% {
    opacity: 0
  }

  100% {
    opacity: 0
  }
}

@-o-keyframes titleAnimation {
  0% {
    opacity: 0;
    -o-transform: translateX(200px);
  }

  8% {
    opacity: 1;
    -o-transform: translateX(0px);
  }

  17% {
    opacity: 1;
    -o-transform: translateX(0px);
  }

  19% {
    opacity: 0;
    -o-transform: translateX(-400px);
  }

  25% {
    opacity: 0
  }

  100% {
    opacity: 0
  }
}

@-ms-keyframes titleAnimation {
  0% {
    opacity: 0;
    -ms-transform: translateX(200px);
  }

  8% {
    opacity: 1;
    -ms-transform: translateX(0px);
  }

  17% {
    opacity: 1;
    -ms-transform: translateX(0px);
  }

  19% {
    opacity: 0;
    -ms-transform: translateX(-400px);
  }

  25% {
    opacity: 0
  }

  100% {
    opacity: 0
  }
}

@keyframes titleAnimation {
  0% {
    opacity: 0;
    transform: translateX(200px);
  }

  8% {
    opacity: 1;
    transform: translateX(0px);
  }

  17% {
    opacity: 1;
    transform: translateX(0px);
  }

  19% {
    opacity: 0;
    transform: translateX(-400px);
  }

  25% {
    opacity: 0
  }

  100% {
    opacity: 0
  }
}

/* Show at least something when animations not supported */
.no-cssanimations .cb-slideshow li span {
  opacity: 1;
}

@media screen and (max-width: 1140px) {
  .cb-slideshow li div h3 {
    font-size: 100px
  }
}

@media screen and (max-width: 600px) {
  .cb-slideshow li div h3 {
    font-size: 40px
  }
}
</style>
