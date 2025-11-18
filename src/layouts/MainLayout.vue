<template>
  <q-layout view="lHh Lpr lFf">
    <div class="header" :class="{ main: $route.meta.IndexPage }">
      <div class="wrapper">
        <router-link to="/" class="home-btn">
          <div class="home-icon">
            <svg
              width="20"
              height="20"
              viewBox="0 0 20 20"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <rect x="2" y="8" width="8" height="8" fill="#0E0E0E" />
              <rect x="10" y="8" width="8" height="8" fill="#0E0E0E" />
              <path d="M6 4L10 0V8H2L6 4Z" fill="#0E0E0E" />
              <path d="M10 0L14 4L18 8H10V0Z" fill="#0E0E0E" />
            </svg>
          </div>
          <span class="font_neodgm_pro font_400">HOME</span>
        </router-link>
        <router-link class="header-logo" to="">
          <h1 class="font_ibm font_400">Yunjeong’s Web Portfolio</h1>
        </router-link>
        <div class="nav">
          <ul>
            <li>
              <router-link to="/about">
                <div class="dot-icon"></div>
                <div class="text-wrapper">
                  <span class="font_neodgm_pro font_400 original-text"
                    >ABOUT</span
                  >
                  <span class="font_neodgm_pro font_400 hover-text">ABOUT</span>
                </div>
              </router-link>
            </li>
            <li>
              <router-link to="">
                <div class="dot-icon"></div>
                <div class="text-wrapper">
                  <span class="font_neodgm_pro font_400 original-text"
                    >WORK</span
                  >
                  <span class="font_neodgm_pro font_400 hover-text">WORK</span>
                </div>
              </router-link>
            </li>
            <li>
              <router-link to="">
                <div class="dot-icon"></div>
                <div class="text-wrapper">
                  <span class="font_neodgm_pro font_400 original-text"
                    >PROJECT</span
                  >
                  <span class="font_neodgm_pro font_400 hover-text"
                    >PROJECT</span
                  >
                </div>
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import router from "src/router";
import { defineComponent } from "vue";
import { RouterLink } from "vue-router";

export default defineComponent({
  name: "MainLayout",

  data() {
    return {};
  },
  methods: {},
  mounted() {},
});
</script>
