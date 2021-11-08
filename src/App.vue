
<script setup lang="ts">
import { nextTick, ref } from "vue";
import ReloadPWA from "./components/ReloadPWA.vue";
import { state } from './globalStore'
import loginStore from '@/pages/login/login'

let install = ref(false);
let deferredPrompt: null

nextTick(() => {

  window.addEventListener("appinstalled", () => {
    console.log("a2hs installed");
  });

  //@ts-ignore
  if (window.matchMedia("(display-mode: standalone)").matches || window.navigator.standalone === true) {
    install.value = false;
    console.log("display-mode is standalone");
  }

  window.addEventListener("beforeinstallprompt", (e) => {
    e.preventDefault();
    //@ts-ignore
    deferredPrompt = e;
    install.value = true;
    console.log("beforeinstallprompt");


    setTimeout(() => {
      install.value = false;
    }, 10000);
  });

})

const installApp = () => {
  install.value = false;
  //@ts-ignore
  deferredPrompt.prompt();

  //@ts-ignore
  deferredPrompt.userChoice.then((choiceResult: any) => {
    if (choiceResult.outcome === "accepted") {
      console.log("User accepted the A2HS prompt");
    } else {
      console.log("User dismissed the A2HS prompt");
    }

    deferredPrompt = null;
  });
}

</script>


<template>
  <ReloadPWA />

  <van-nav-bar
    v-show="loginStore.state.auth"
    class="mb-2 elevation-2"
    :title="state.title"
    left-arrow
    @click-left="$router.back()"
  />

  <!-- <router-link to="home">Home</router-link> -->

  <div v-show="install" class="backgroundBar divInstall">
    <span>Instalar KmLivre</span>
    <button @click="installApp()" dark color="blue" class="ml-10">Instalar</button>
  </div>

  <router-view></router-view>
</template>


<style lang='scss'>
@import "./assets/index.scss";
@import "./plugins/xModal/xModal.css";
@import "./plugins/xGridV2/xGridV2.css";

// #app {
//   font-family: Avenir, Helvetica, Arial, sans-serif;
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   text-align: center;
//   color: #2c3e50;
//   margin-top: 60px;
// }

.divInstall {
  background-color: rgb(145 0 0);
  top: -1px;
  color: #ffffff;
  /* height: 14px; */
  left: 50%;
  padding: 8px;
  position: fixed;
  text-align: center;
  transform: translate(-50%);
  /* width: 100%; */
  z-index: 1000;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
}
</style>
