<script setup>
import { ref } from "vue";
import { collection, addDoc } from "firebase/firestore";
import { db, auth } from "../firebase";

const text = ref('');

const addText = () => {
  console.log(text.value);
  if(!text.value.trim()){
    text.value = "";
    return console.log("Mostrar alerta de mensaje vacio");
  }
  addDoc(collection(db, "chats"), {
    text: text.value,
    uid: auth.currentUser.uid,
    time: Date.now(),
    displayName: auth.currentUser.displayName,
  })
    .then(() => {
      console.log('aqui iria un sonido');
      text.value = '';
    })
    .catch((e) => console.log(e));
};
</script>

<template>
  <q-footer elevated class="bg-grey-8 text-white">
    <q-toolbar>
      <q-input
        class="full-width"
        dark
        dense
        standout
        label="Ingrese texto"
        v-model="text"
        @keyup.enter="addText"
        >
        <template #append>
          <q-icon name="send" class="cursor-pointer" type="submit" @click="addText"/>
        </template>
      </q-input>
    </q-toolbar>
  </q-footer>
</template>
