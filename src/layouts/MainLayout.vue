<template>
  <q-layout view="hHr lpR fFr">

    <Navbar />

    <q-page-container>
      <!-- {{userGoogle}} -->
      <router-view />
    </q-page-container>

    <FooterAdd v-if="userGoogle"/>

  </q-layout>
</template>

<script setup>
import { inject } from 'vue';
import Navbar from 'src/components/Navbar.vue';
import FooterAdd from 'src/components/FooterAdd.vue'

const userGoogle = inject('userGoogle');
</script>
