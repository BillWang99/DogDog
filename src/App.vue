<script setup>
  import axios from 'axios';
  import { onMounted, ref } from 'vue';
  import LoaderElement from './components/LoaderElement.vue';

  const dogData = ref(null);
  const loader = ref(false);

  const fetchData = async() => {
    loader.value = true;
    var imgElement = document.getElementById('dogImg');
    if(imgElement != null){
      imgElement.setAttribute('hidden', '');
    }
    try{
      const response = await axios.get('https://api.thedogapi.com/v1/images/search?limit=1',{
        headers:{
          'x-api-key': 'live_hFTx9eBIvmadjJvU0XfQOmCJmvJZepmUJUSkW2cT3cYEhPQShQetivMhdpyWmgwZ'
        }
      });

      if(response.status ===200){
        dogData.value = response.data;
        loader.value = false;
        if(imgElement != null){
          imgElement.removeAttribute('hidden');
        }
      }
    } catch(error){
      console.log(error);
    }
  }

  // onMounted(()=>{
  //   fetchData();
  // })
</script>

<template>

    <header>
      <nav class="navbar bg-warning-subtle" data-bs-theme="dark">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">DogDog好運狗狗</a>
        </div>
      </nav>
    </header>
    
    <main>
      <h2 class="text-center mt-3">看看這次會遇到哪隻好運狗狗</h2>
      <div class="d-grid gap-1 w-25 col-6 mx-auto">
        <button class="btn btn-outline-warning" @click="fetchData()">碰運氣</button>
      </div>
      <div id="imgArea" v-if="dogData">
        <img :src="dogData[0].url" id="dogImg" class="mt-5">
      </div>

      <div v-if="loader">
        <LoaderElement/>
      </div>
    </main>
    <footer>
      <p>好運狗狗</p>
    </footer>

  
</template>

<style scoped>
  main { 
    flex: 1;
    padding: 10px;
  } 

  footer { 
    background-color: #494949;
    padding: 10px;
    text-align: center;
    color:white;
  }

  #imgArea{
    text-align: center;
  }
  #dogImg{
    width: 50%;
    height: auto;
    
  }
</style>
