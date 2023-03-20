<script setup>

import { ref, onMounted, computed } from 'vue';
import ButtonCounter from './components/ButtonCounter.vue'
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue'

const posts = ref([])
const postXPage = 10
const inicio = ref(0)
const fin = ref(postXPage)
const loading = ref(true)

const favorito = ref('')
const cambiarFavorito = (title) => {
    favorito.value = title
}

const prev = () => {
    inicio.value += -postXPage
    fin.value += -postXPage
}

const next = () => {
    inicio.value += +postXPage
    fin.value += +postXPage
}

/*
onMounted(async() => {
    try {
        const resp = await fetch('https://jsonplaceholder.typicode.com/posts')
        posts.value = await resp.json()
    } catch (error) {
        console.log(error);
    }finally{
        loading.value = false
    }
})
*/
/*
fetch('https://jsonplaceholder.typicode.com/posts')
    .then(res => res.json())
    .then((data) => {
        posts.value = data
    })
    .finally(() => loading.value = false)
*/

const fetchData = async () => {
    try {
        const resp = await fetch('https://jsonplaceholder.typicode.com/posts')
        posts.value = await resp.json()
    } catch (error) {
        console.log(error);
    }finally{
        loading.value = false
    }
}

fetchData()

const maxLength = computed(() => posts.value.length)
</script>

<template>
  <LoadingSpinner v-if="loading"/>
    <div class="container" v-else>
        <h1>APP</h1>
        <h2>Mi post favorito: {{ favorito }}</h2>
    
        <PaginatePost @next="next" 
                      @prev="prev" 
                      :inicio="inicio" 
                      :fin="fin"
                      :maxLength="posts.length"
                      class="mb-2" />
    
        <BlogPost v-for="post in posts.slice(inicio, fin)" 
                  :key="post.id" 
                  :title="post.title" 
                  :id="post.id" 
                  :body="post.body" 
                  :cambiarFavorito="cambiarFavorito" 
                  class="mb-2" />
    </div>
</template>