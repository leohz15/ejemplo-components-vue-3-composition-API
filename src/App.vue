<script setup>
import { ref, onMounted } from 'vue';

import BlogPost from './components/BlogPost.vue';
import PagesPost from './components/PagesPost.vue';
import loadingSpinner from './components/loadingSpinner.vue';

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const favorito = ref('');
const loading = ref(true);

const cambiarFavorito = (title) => { 
  favorito.value = title;
}

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
}

const prev = () => {
  inicio.value += -postXpage;
  fin.value += -postXpage;s
}

onMounted(() => {
  fetchData();
});

// onMounted(async() => {
// loading.value = true;

//     try {
//       const res = await fetch('https://jsonplaceholder.typicode.com/posts')
//       posts.value = await res.json()
//     } catch (error) {
//       console.log(error)
//     } finally {
//       setTimeout(() => {
//         loading.value = false;
//       }, 2000)
//     }

// })

// fetch('https://jsonplaceholder.typicode.com/posts')
//   .then(res => res.json())
//   .then((data) => posts.value = data)
//   .catch(e => console.log(e))
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false
//     }, 2000)
//   });

async function fetchData() {
try {
const res = await fetch('https://jsonplaceholder.typicode.com/posts');
posts.value = await res.json();
} catch (error) {
console.log(error);
} finally {
setTimeout(() => {
loading.value = false;
}, 2000);
}
}

fetchData();
  

</script>

<template>

<loadingSpinner v-if="loading" />

  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi post favorito: {{favorito}}</h2>

  <PagesPost 
  @next="next" 
  @prev="prev" 
  class="mb-2"
  :inicio="inicio"
  :fin="fin"
  :maxLength="posts.length"
  />
    
    

    <BlogPost 
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title" 
      :id="post.id" 
      :body="post.body" 
      @cambiarFavoritoNombre="cambiarFavorito"
      class="mb-2"
    ></BlogPost>

  </div>
</template>