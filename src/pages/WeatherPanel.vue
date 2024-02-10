<template>
  <div class="q-pa-md q-gutter-sm">
    <q-banner rounded :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
      <template v-slot:avatar>
        <img
          src="https://cdn.quasar.dev/img/mountains.jpg"
          style="width: 100px; height: 64px"
        />
      </template>

      Lorem ipsum dolor sit amet consectetur adipisicing elit. At officiis
      voluptate dolore cupiditate inventore quaerat, totam optio. Labore
      voluptas incidunt officiis quasi, itaque dolorem! Sequi quo quam
      voluptatibus nostrum sed?
    </q-banner>
  </div>

  <div class="q-pa-md q-gutter-md">
    <q-btn color="primary" @click="showTextLoading"> Show it </q-btn>

    <q-card class="relative-position card-example" flat bordered>
      <q-card-section class="q-pb-none">
        <div class="text-h6">Lorem Ipsum</div>
      </q-card-section>

      <q-card-section>
        <transition
          appear
          enter-active-class="animated fadeIn"
          leave-active-class="animated fadeOut"
        >
          <div v-show="showSimulatedReturnData">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent
            vel magna eu risus laoreet tristique. Nulla ut fermentum elit, nec
            consequat augue. Morbi et dolor nec metus tincidunt pellentesque.
            Nullam non semper ante. Fusce pellentesque sagittis felis quis
            porta. Aenean condimentum neque sed erat suscipit malesuada. Nulla
            eget rhoncus enim. Duis dictum interdum eros.
          </div>
        </transition>
      </q-card-section>

      <q-inner-loading :showing="visible">
        <q-spinner-gears size="50px" color="primary" />
      </q-inner-loading>
    </q-card>
  </div>
</template>

<script>
import { ref } from "vue";

// Don't forget to specify which animations
// you are using in quasar.config file > animations.
// Alternatively, if using UMD, load animate.css from CDN.
export default {
  setup() {
    const visible = ref(false);
    const showSimulatedReturnData = ref(false);

    return {
      visible,
      showSimulatedReturnData,

      showTextLoading() {
        visible.value = true;
        showSimulatedReturnData.value = false;

        setTimeout(() => {
          visible.value = false;
          showSimulatedReturnData.value = true;
        }, 3000);
      },
    };
  },
};
</script>

<style lang="sass" scoped>
.card-example
  width: 288px
  height: 290px
</style>
