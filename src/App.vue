<script setup>
import { onMounted, ref } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const miFavorito = ref("");

const cambiarFavorito = (title) => {
  miFavorito.value = title;
};

const next = () => {
  inicio.value += postXpage;
  fin.value += postXpage;
};
const preview = () => {
  inicio.value -= postXpage;
  fin.value -= postXpage;
};

onMounted(async () => {
  //loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    setTimeout(() => {
      //setTimeout para simular el tiempo de carga, en produccion se quita
      loading.value = false;
    }, 2000);
    //loading.value = false;
  }
});

// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then((res) => res.json())
//   .then((data) => {
//     posts.value = data;
//   })
//   .finally(() => {
//     loading.value = false;
//   });

// .then((data) => console.log(data));
</script>

<template>
  <div class="card" style="width: 70rem">
    <LoadingSpinner v-if="loading" />
    <div class="container" v-else>
      <h1 class="text-center mb-2">
        Mi Post favorito: {{ miFavorito || "Sin favorito" }}
      </h1>

      <PaginatePost
        @next="next"
        @preview="preview"
        :inicio="inicio"
        :maxlength="posts.length"
        :fin="fin"
        class="mb-2"
      />

      <!-- <PaginatePost class="mb-2" @click="next"  /> -->

      <div>
        <BlogPost
          v-for="post in posts.slice(inicio, fin)"
          :key="post.title"
          :title="post.title"
          :id="post.id"
          :body="post.body"
          class="mb-2"
          :cambiarFavorito="cambiarFavorito"
        />
      </div>
    </div>
  </div>
</template>
