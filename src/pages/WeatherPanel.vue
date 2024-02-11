<template>
  <div class="q-pa-md q-gutter-sm">
    <q-banner rounded :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-2'">
      <template v-slot:avatar>
        <img
          src="https://cdn.quasar.dev/img/mountains.jpg"
          style="width: 100px; height: 64px"
        />
      </template>

      Lorem ipsum dolor sit amet
    </q-banner>
  </div>

  <div style="display: flex; justify-content: center">
    <div class="q-pa-md q-gutter-md">
      <q-card class="relative-position card-example" flat bordered>
        <q-card-section class="q-pb-none">
          <div class="text-h6">Weather</div>
        </q-card-section>

        <q-card-section>
          <transition
            appear
            enter-active-class="animated fadeIn"
            leave-active-class="animated fadeOut"
          >
            <div v-show="showSimulatedReturnData">
              {{ textFieldValue }}
            </div>
          </transition>
        </q-card-section>

        <q-inner-loading :showing="visible">
          <q-spinner-gears size="50px" color="primary" />
        </q-inner-loading>
      </q-card>
    </div>

    <div class="q-pa-md" style="max-width: 400px">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="name"
          label="Longitude *"
          hint="Example: 44.34"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="age"
          label="Latitude *"
          hint="Example: 10.99"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <div>
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </div>
  </div>
</template>

<script>
import { useQuasar } from "quasar";
import { ref } from "vue";
import { api } from "boot/axios";

export default {
  setup() {
    const visible = ref(false);
    const showSimulatedReturnData = ref(false);
    const $q = useQuasar();
    const name = ref(null);
    const age = ref(null);
    const data = ref(null);

    function loadData() {
      api
        .get(
          "https://api.openweathermap.org/data/2.5/weather?lat=44.34&lon=10.99&appid=30292a6a5232adf186142c5e87134704"
        )
        .then((response) => {
          data.value = response.data;
          console.log(data.value);
        });
    }

    function onSubmit() {
      api
        .get(
          "https://api.openweathermap.org/data/2.5/weather?lat=44.34&lon=10.99&appid=30292a6a5232adf186142c5e87134704"
        )
        .then((response) => {
          data.value = response.data;
          this.textFieldValue = "data.value.formattedString;";
          console.log(data.value);
        });

      $q.notify({
        color: "green-4",
        textColor: "white",
        icon: "cloud_done",
        message: "Processing",
      });

      visible.value = true;
      showSimulatedReturnData.value = false;

      setTimeout(() => {
        visible.value = false;
        showSimulatedReturnData.value = true;
      }, 3000);
    }

    return {
      visible,
      showSimulatedReturnData,
      onSubmit,
      loadData,
      name,
      age,
      data,
      textFieldValue: "adasds",
      onReset() {
        name.value = null;
        age.value = null;
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
