<script>
import axios from 'axios';

export default {
    props: {
        sort: {
            type: String
        },
        mode: {
            type: String
        },
        posts: {
            type: Array
        }
    },
    data() {
        return {
            stateMode: '',
        };
    },
    mounted() {

        // this.filterYearPosts = this.sortYear( this.posts, this.sort );
    },
    watch: {
        sort: function (val, oldVal){
            this.filterYearPosts = this.sortYear( this.posts, val );
        },
    },
    methods: {
        sortYear( posts, sort = 'ASC' ) {
            let filter =  posts.sort( (a,b) => {
                if( a.postmeta.year[0] === b.postmeta.year[0]) {
                    return sort == 'ASC' ? a.postmeta.position[0] - b.postmeta.position[0] : b.postmeta.position[0] - a.postmeta.position[0]
                } else {
                    return sort == 'ASC' ? a.postmeta.year[0] - b.postmeta.year[0] : b.postmeta.year[0] - a.postmeta.year[0]
                }
            })
            return filter;
        },
        getImagePost( post ) {
            return post.image.large;
        }
    },
}


</script>
<template>
        <!-- {{posts}} -->
    <div v-if="posts.length" class="river__list">

        <div v-for="post in sortYear(posts, sort)" :key="post.postmeta.creation_date" class="river__item" >
            <div class="river__image"> <img :src="getImagePost(post)" alt=""> </div>
            <div class="river__year">{{post.postmeta.year[0]}}</div>
            <div class="river__text">{{post.post_content}}</div>
        </div>

    </div>
</template>


<style scoped>
.river__item {
    display: flex;
    gap: 15px;
    align-items: center;
}
.river__image {
    border-radius: 10px;
    overflow: hidden;
}
.river__image img{
    width: 150px;
}
</style>