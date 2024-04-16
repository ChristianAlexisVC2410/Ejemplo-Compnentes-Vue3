<script setup>
import {ref,computed, onMounted} from 'vue';
import BlogPost from './components/BlogPost.vue';
import ButtonCounter from './components/ButtonCounter.vue';
import PaginationPost from './components/PaginationPost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';


/*const posts=ref([
  {title:"Post 1",id:1,body:"descripcion 1"},
  {title:"Post 2",id:2,body:"descripcion 2"},
  {title:"Post 3",id:3,body:"descripcion 3"},
  {title:"Post 4",id:4}
]);*/

const posts=ref([]);
const postXpage=10;
const inicio=ref(0);
const fin=ref(postXpage);
const loading=ref(true);

const next=()=>{
  inicio.value=inicio.value + postXpage;
  fin.value=fin.value + postXpage;
}

const prev=()=>{
  inicio.value=inicio.value - postXpage;
  fin.value=fin.value - postXpage;
}


const favorito=ref("");
const cambiarFavorito=(title)=>{
favorito.value=title;
};

onMounted(async()=>{
  //loading.value=true;
  try{
    const res =await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value=await res.json();
  }catch(error){
    console.log(error);
  }finally{
    loading.value=false;
  }
});

/*fetch('https://jsonplaceholder.typicode.com/posts')
.then(res=>res.json())
.then(data=>posts.value=data)
.catch((e)=>console.log(e))
.finally(()=>{
  setTimeout(()=>{
    loading.value=false
  },2000)
});*/



const maxLegth=computed(()=>posts.value.length);

</script>

<template>

    <LoadingSpinner v-if="loading"/>


  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>

    <PaginationPost class="my-2" @next="next" @prev="prev" :inicio="inicio" :fin="fin" :maxLegth="maxLegth"/>
    <BlogPost class="mt-2" v-for="p in posts.slice(inicio,fin)" 
    :key="p.id" :title="p.title" :id="p.id" :body="p.body" colorText="primary"
    @cambiarFavoritoNombre="cambiarFavorito"/>
  </div>
 
</template>

<style scoped>


</style>
