<template>
  <div class="home">
    <MyNavbar/>
<div class="backgroundSite">
</div>
    <div class="myfilms">
      <div class="accordion" id="accordionExample">
        <div class="accordion-item">
          <h2 class="accordion-header" id="headingOne">
            <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
              Самое просматриваемое видео:
            </button>
          </h2>
        </div>
      </div>
      <div class="card" style="" v-for="film of vuefilms" v-if="Math.max(...vueViewsFilms) === film.views">
          <div class="card-body">
            <h5 class="card-title" @click="write()">Название фильма: {{ film.name }}</h5>
            <h5 class="card-title">Режиссёр: {{ film.director }}</h5>
            <h5 class="card-title">Дата: {{ film.date }}</h5>
            <h5 class="card-title">Просмотры: {{ film.views }}</h5>
            <h5 class="card-title">Популярность: {{ film.popularity }}</h5>
            <p class="card-text">Описание: {{ film.description }}.</p>
            <router-link class="btn btn-primary" :to="{ path: '/detailoffilmw', query: { url: film.url, author: film.author, name: film.name, views: film.views, poster: film.poster, filmKey: film.filmKey  }}">
              Play Video
            </router-link>
            <details>
              <summary>Предпросмотр</summary>
              <video :poster="film.poster" width="400" height="300" controls="controls">
                <source :src="film.url">
                Видео недоступно по разрешению правообладателя.
              </video>
            </details>
              
          </div>
        </div>
      <!-- <div>{{ Math.max(...vueViewsFilms) }}</div> -->
      
      
      <div class="accordion" id="accordionExample">
        <div class="accordion-item">
          <h2 class="accordion-header" id="headingOne">
            <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
              Список всех опубликованных видео:
            </button>
          </h2>
        </div>
      </div>
      <!-- :key="film" -->
        <div class="card" style="" v-for="film of vuefilms">
          <div class="card-body">
            <h5 class="card-title" @click="write()">Название фильма: {{ film.name }}</h5>
            <h5 class="card-title">Режиссёр: {{ film.director }}</h5>
            <h5 class="card-title">Дата: {{ film.date }}</h5>
            <h5 class="card-title">Просмотры: {{ film.views }}</h5>
            <h5 class="card-title">Популярность: {{ film.popularity }}</h5>
            <p class="card-text">Описание: {{ film.description }}.</p>
            <router-link class="btn btn-primary" :to="{ path: '/detailoffilmw', query: { url: film.url, author: film.author, name: film.name, views: film.views, poster: film.poster, filmKey: film.filmKey  }}">
              Play Video
            </router-link>
            <details>
              <summary>Предпросмотр</summary>
              <video :poster="film.poster" width="400" height="300" controls="controls">
                <source :src="film.url">
                Видео недоступно по разрешению правообладателя.
              </video>
            </details>
              
          </div>
        </div>
        <MyFooter/>
    </div>    
  </div>
</template>
<script src="https://unpkg.com/vuefire@1.3.0/dist/vuefire.js"></script>
<script src="https://www.gstatic.com/firebasejs/3.4.0/firebase.js"></script>
<script src="https://www.gstatic.com/firebasejs/3.1.0/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/3.1.0/firebase-database.js"></script>
<script>
import firebase from 'firebase/app';
import * as fb from 'firebase';
import MyNavbar from '@/components/MyNavbar.vue';
import MyFooter from '@/components/MyFooter.vue';

const vuefilms = [];
var vuePopularfilm = [];
// let firebaseConfig = {
//   apiKey: "AIzaSyAs2iNICjyyGgXQvtRTtYt67712RCcFOVs",
//   authDomain: "listoffilms-8536f.firebaseapp.com",
//   databaseURL: "https://listoffilms-8536f-default-rtdb.firebaseio.com",
//   projectId: "listoffilms-8536f",
//   storageBucket: "listoffilms-8536f.appspot.com",
//   messagingSenderId: "595539552049",
//   appId: "1:595539552049:web:1ba327027936ede9ff3210"
// };
// firebase.initializeApp(firebaseConfig);

var myFirebaseConfig = {
    apiKey: "AIzaSyAhI25hD5Yv1eHaDsOEb6J7pAPFSCagSTQ",
    authDomain: "vueusers-d7a7e.firebaseapp.com",
    databaseURL: "https://vueusers-d7a7e-default-rtdb.firebaseio.com",
    projectId: "vueusers-d7a7e",
    storageBucket: "vueusers-d7a7e.appspot.com",
    messagingSenderId: "222841222603",
    appId: "1:222841222603:web:c935d47616057664b9724e"
  };
  // Initialize Firebase
firebase.initializeApp(myFirebaseConfig);
var database = firebase.database()
export default {
  name: 'Home',
  components: {
    MyNavbar,
    MyFooter
  },
  methods:{
    write(){
      console.log(this.vuePopularF)
      console.log(this.vueViewsFilms)
    }
  },
  data:()=>({
    vuefilms,
    vuePopularF:'',
    vueViewsFilms:[]
}),
  

  // mounted(){
  //   database.ref('/films').on('value', snapshot => {
  //     snapshot.forEach(async (oneFilm) => {
  //       let myfilm = await oneFilm.val();
  //       this.vuefilms.push(myfilm)
  //     })
  //   });
  // },
  // надо использовать код выше а не ниже но так не загружаются все видео
  mounted(){
    let cursorOfFilms = 0
    database.ref('/films').on('value', snapshot => {
      snapshot.forEach((oneFilm) => {
        
        let myfilm = oneFilm.val();
        // vuefilms[a++].push({"filmKey": oneFilm.key})

        //let nameOfUserWhichPublishedThisVideo = oneFilm.val().aut
        this.vuefilms.push(myfilm)
        this.vueViewsFilms.push(myfilm.views)
        vuefilms[cursorOfFilms].filmKey = oneFilm.key
        // console.log(vuefilms[cursorOfFilms])
        cursorOfFilms++;
        // if(cursorOfFilms < 3){
        //   Number(vuefilms[cursorOfFilms].views) >= Number(vuefilms[cursorOfFilms + 1].views) ? vuePopularfilm = Number(vuefilms[cursorOfFilms]) : vuePopularfilm = vuePopularfilm;

        // }
        // console.log('vuePopularfilm', vuePopularfilm)
      })
    });
    // this.vuePopularF = Math.max(this.vuefilms[0].views)
    vuefilms.forEach((film, index) => {
      if(index <  vuefilms.length - 1){
        //vuePopularfilm = film
        // return Number(film.views) > Number(vuefilms[index + 1].views)
        //console.log(vuePopularfilm)
        // let vuePopularfilms = Math.max(vuefilms[index].views, vuefilms[index + 1].views)
        
      }
      
    })
    
    
  },
  beforeDestroy(){
    this.vuefilms = []
  }
}
</script>
<style scoped>
  
</style>