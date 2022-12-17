<script setup>
import { ref, provide } from "vue";
import { onAuthStateChanged } from "firebase/auth";
import { auth } from "./firebase";
import { useQuasar } from "quasar";

const userGoogle = ref(false);
provide('userGoogle', userGoogle);
const $q = useQuasar();

onAuthStateChanged(auth, (user) => {
  userGoogle.value = user;
  // console.log(user);
  setTimeout(() => {
    $q.loading.hide();
  }, 1000)
});


$q.loading.show();

</script>

<template>
  <router-view v-if="userGoogle !== false"/>
</template>

<style>
body {
  overflow-y: hidden;
}
</style>

