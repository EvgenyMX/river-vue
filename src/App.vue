<script setup>
  import RiverPosts from './components/RiverPosts.vue'
  import SortingTabs from './components/SortingTabs.vue'
  import FormNewRive from './components/FormNewRive.vue'
  import { reactive, computed, onMounted, ref } from 'vue'
  import axios from 'axios';

  let sort = ref('ASC')
  let mode = ref('view')

  const river = reactive({
    sort: 'ASC',
    mode: 'view',
    posts: [],
    filterYearPosts: [],
  })


  const sortButton = [
    {
      'key': 0,
      'sort': 'ASC',
      'class': '',
      'name': 'Сначала новые',
      'click': ''
    },
    {
      'key': 1,
      'sort': 'DESC',
      'class': '',
      'name': 'Сначала старые',
      'click': ''
    }
  ]

  onMounted(() => {
    getPosts()// <div>
  })


  const getPosts = () => {
    axios.get('https://me-dev.test.nasledie.digital/wp-json/ndriver/v1/river/get')
      .then( resp => {
          if ( resp.status != 200 ) {
              console.log(`Error Code: ${resp.status}`);
          }
          river.posts = resp.data;
      })
      .catch( error => {
          console.log('Error', error);
          return [];
      })
  }

  const sortValue = ( s ) => {
    return river.sort = s;
  }
  const stateMode = () => {
    return river.mode == 'view' ? river.mode = 'edit' : river.mode = 'view';
  }
</script>
<template>
  <div class="river-content">
    <header class="river-header">
      <div class="river-title">
        <h1 >Река жизни</h1>
        <div class="river-title__helper title-helper">
          <i class="title-helper__icon">i</i>
          <p class="title-helper__text">
            В этом разделе мы выделяем наиболее значимые события жизни героя капсулы или истории семьи, коротко описываем их и располагаем на временной шкале по годам. Два фильтра вверху "Сначала старые" и "Сначала новые" и шкала лет слева позволяют быстро менять порядок представления событий, легко перемещаться по ним и представлять историю в динамике.
          </p>
        </div>
      </div>
      <div class="river-buttons">
        <button>C</button>
        <button @click="stateMode" v-if="river.mode == 'view'"> Edit </button>
        <button @click="stateMode" v-else> Save </button>
      </div>
    </header>

    <div class="river-sorting">
      <button v-for="btn in sortButton" :key='btn.key' @click="river.sort=btn.sort" :class="{'sorting--active': btn.sort == river.sort}">{{btn.name}}</button>
    </div>

    <div class="river-new" v-if="river.mode == 'edit'">
      <FormNewRive />
    </div>
    <div class="river-posts">
      <div class="river-vertical-line"></div>
      <div class="river-items">
        <RiverPosts :posts="river.posts" :sort='river.sort' :mode="river.mode"/>
      </div>
    </div>

  </div>
</template>

<style scoped>


.river-content {
  width: 100%;
}

.river-header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
  top: 0;
  right: 0;
}

.river-title {
  position: relative;
  display: flex;
  align-items: center;
  gap: 15px;
}
.title-helper {
  position: relative;
}

.title-helper__icon {
  border: 1px solid #444444;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-content: center;
  width: 25px;
  height: 25px;
  box-shadow: 0 2px 12px rgb(0 0 0 / 12%);

  transition: 5s;

}
.title-helper__icon:hover {
  transition: 5s;
  cursor: pointer;
}
.title-helper__text {
  position: absolute;
  top: 0;
  left: 150%;
  display: none;
  max-width: 380px;
  width: max-content;
  padding: 5px 10px;
  border-radius: 10px;
  transition: 5s;
  background: #ffffff;
  box-shadow: 0 2px 12px rgb(0 0 0 / 12%);
}
.title-helper__icon:hover + .title-helper__text{
  display: block;
  transition: 5s;
}


.river-buttons {
  display: flex;
  flex-direction: row;
  gap: 15px;
}
.river-header button{
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;

  width: 45px;
  height: 45px;

  background: transparent;
  border: none;

  box-shadow: 0 2px 12px rgb(0 0 0 / 12%);
  border-radius: 50px;

  transition: 0.3s;
}
.river-header button:hover{
  opacity: 0.7;

}
.river-header button:active{
  opacity: 0.7;

}


.river-sorting {
  display: flex;
  justify-content: center;
  gap: 20px;
  width: 100%;
}
.river-sorting button{

    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    background: transparent;
    border: none;

    font-size: 18px;
    color: #B7BDCE;
    transition: 0.3s;
}
.river-sorting button.sorting--active{
    color: #444444;
}
.river-sorting button:hover{
    color: #444444;
}


</style>
..