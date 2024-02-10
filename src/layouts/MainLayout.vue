<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <div>
        <div>
          <q-img src="~assets/Back.jpg" class="opaque-img"></q-img>
          <q-parallax :height="150" width="100">
            <q-toolbar>
              <q-btn
                flat
                dense
                round
                icon="menu"
                aria-label="Menu"
                @click="toggleLeftDrawer"
              />
            </q-toolbar>

            <div class="text-h3">Weather App</div>
            <div class="text-subtitle2">{{ todaysDate }}</div>

            <template v-slot:media>
              <video
                class="header-image"
                poster="~assets/CloudsImage.jpg"
                autoplay
                loop
                muted
              >
                <source type="video/mp4" src="~assets/BackVideo.mp4" />
              </video>
            </template>
          </q-parallax>
        </div>
      </div>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="220"
      :breakpoint="600"
    >
      <q-scroll-area
        style="
          height: calc(100% - 216px);
          margin-top: 216px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="info" />
            </q-item-section>

            <q-item-section> About </q-item-section>
          </q-item>

          <q-item to="/weatherPanel" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="cloud" />
            </q-item-section>

            <q-item-section> Weather panel</q-item-section>
          </q-item>

          <q-item to="https://openweathermap.org/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="link" />
            </q-item-section>

            <q-item-section> OpenWeather website</q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img
        class="absolute-top"
        src="~assets/CloudsImage.jpg"
        style="height: 216px"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="85px" class="q-mb-sm">
            <img src="~assets/Avatar.jpg" />
          </q-avatar>
          <div class="text-weight-bold">Hlib Monastyrov</div>
          <div>NTU "DP" 122-21-1</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>
  </q-layout>
</template>

<script>
import { date } from "quasar";
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    const leftDrawerOpen = ref(false);
    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
  computed: {
    todaysDate() {
      let timeStamp = Date.now();
      let formattedString = date.formatDate(timeStamp, "dddd D MMMM");
      return formattedString;
    },
  },
});
</script>

<style lang="scss">
.header-image {
  filter: blur(3px);
  z-index: -2;
}
.opaque-img {
  opacity: 0.4;
  width: 100%;
  height: 100%;
  z-index: -1;
  position: absolute;
}
</style>
