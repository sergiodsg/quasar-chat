<script setup>
import { ref, inject, nextTick, watchEffect } from "vue";
import { collection, query, onSnapshot, orderBy, limit } from "firebase/firestore";
import { db, auth } from "../firebase";

const userGoogle = inject("userGoogle");
const messages = ref([]);
const chatRef = ref(null);

let inicio = 0;

watchEffect((onCleanup) => {
  if(userGoogle.value){
    const q = query(collection(db, "chats"), orderBy("time", "desc"), limit(10));
    const unsubscribe = onSnapshot(q, async(snapshot) => {
        snapshot.docChanges().forEach((change) => {
          if(change.type === "added"){
            console.log("New chat: ", change.doc.data());
            messages.value.push({
              id: change.doc.id,
              ...change.doc.data()
            });
          }
        });

        if(inicio === 0) {
          messages.value = messages.value.reverse();
          inicio = 1;
        }

        await nextTick();
        console.log(chatRef.value.scrollTop);
        chatRef.value.scrollTo(0, chatRef.value.scrollHeight);
    });

    onCleanup(unsubscribe);
  }
});
</script>

<template>
  <q-page v-if="!userGoogle">
    <h3 class="text-center text-primary">Debes iniciar sesion</h3>
  </q-page>
  <q-page v-else padding>
    <div class="q-pa-md row justify-center scrollChat" ref="chatRef">
      <div style="width: 100%; max-width: 400px">
        <template
          v-for="message in messages"
          :key="message.id"
        >
          <q-chat-message :text="[message.text]" :sent="message.uid === auth.currentUser.uid" :name="message.displayName"/>
        </template>
      </div>
    </div>
  </q-page>
</template>

<style>
.scrollChat {
  height: calc(100vh - (60px + 60px));
  overflow-y: scroll;
}
</style>
