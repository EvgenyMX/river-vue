<script>
import axios from 'axios';

export default {
    props: {
        sort:{
            type: String
        }
    },
    data() {
        return {
            posts: [],
            filterYearPosts: [],
            activeSorting: 'ASC',

        };
    },
    // setup(props) {
    //     console.log(props.sort)
    // },
    mounted() {
        this.getPosts();
    },
    watch: {
        sort: function (val, oldVal){
            this.filterYearPosts = this.sortYear( this.posts, val );
        }
    },
    // computed: {
    //     updateSort() {
    //         console.log( this.sort );
    //         this.filterYearPosts = this.sortYear( this.posts, this.sort );
    //     }
    // },
    methods: {
        getPosts() {
            axios.get('https://me-dev.test.nasledie.digital/wp-json/ndriver/v1/river/get')
                .then( resp => {
                    if ( resp.status != 200 ) {
                        console.log(`Error Code: ${resp.status}`);
                        // return;
                    }
                    this.posts = resp.data;
                    this.filterYearPosts = this.sortYear( this.posts, this.activeSorting );
                    return resp.data;
                })
                .catch( error => {
                    console.log('Error', error);
                })
        },
        sortYear( posts, sort = 'ASC' ) {
            let filter =  posts.sort( (a,b) => {
                if( a.postmeta.year[0] === b.postmeta.year[0]) {
                    return sort == 'ASC' ? a.postmeta.position[0] - b.postmeta.position[0] : b.postmeta.position[0] - a.postmeta.position[0]
                } else {
                    return sort == 'ASC' ? a.postmeta.year[0] - b.postmeta.year[0] : b.postmeta.year[0] - a.postmeta.year[0]
                }
            })
            return filter;
        }
    },
}


</script>
<template>
    <div v-if="posts.length" class="river__list">

        <div v-for="post in filterYearPosts" :key="post.postmeta.creation_date" class="river__item" >
            <div class="river__image">{{sort}}</div>
            <div class="river__year">{{post.postmeta.year[0]}}</div>
            <div class="river__text">{{post.post_content}}</div>
        </div>

    </div>
</template>