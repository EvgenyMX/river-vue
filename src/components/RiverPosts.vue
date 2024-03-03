<script setup>
import axios from 'axios';
import { watch } from 'vue';

const props = defineProps({
    sort: String,
    mode: String,
    posts: String,
})

// watch( props.sort, ( val, oldVal ) => {
//     this.filterYearPosts = this.sortYear( this.posts, val );
// } )

const sortYear = ( posts, sort = 'ASC' ) => {
    let filter =  posts.sort( (a,b) => {
        if( a.postmeta.year[0] === b.postmeta.year[0]) {
            return sort == 'ASC' ? a.postmeta.position[0] - b.postmeta.position[0] : b.postmeta.position[0] - a.postmeta.position[0]
        } else {
            return sort == 'ASC' ? a.postmeta.year[0] - b.postmeta.year[0] : b.postmeta.year[0] - a.postmeta.year[0]
        }
    })
    return filter;
}
const getImagePost = ( post ) => {
    return post.image.large;
}


const removePost = ( post_id, index ) => {

    axios.post('https://me-dev.test.nasledie.digital/wp-json/ndriver/v1/river/remove',
            {
                post_id: post_id
            }
        )
    .then( resp => {
        props.posts.splice(index, 1);
    })
}



</script>
<template>
        <!-- {{posts}} -->
    <div v-if="posts.length" class="river__list">

        <div v-for="( post, index ) in sortYear(posts, sort)" :data-post_id="post.ID" :key="post.postmeta.creation_date" class="river__item" >
            <div class="river__image"> <img :src="getImagePost(post)" alt=""> </div>
            <div class="river__year">{{post.postmeta.year[0]}}</div>
            <div class="river__text">{{post.post_content}}</div>
            <div class="">
                <QuillEditor theme="snow" />
            </div>
            <button class="river__btn--remove" @click="removePost(post.ID, index)">Удалить</button>
        </div>

    </div>
</template>


<style scoped>

</style>