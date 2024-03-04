<script setup>
  import axios from 'axios';
  import { reactive, computed, onMounted, watch, ref } from 'vue'
  // import { quillEditor, Quill } from 'vue3-quill'
  // import customQuillModule from 'customQuillModule'
  const riverForm = reactive({
    year: null,
    history: 0,
    text: null,
    preview: null,
  })
  const props = defineProps({
    defaultImage: String,
    posts: String,
  })

  const addRiverPost = () => {
    console.log( riverForm );
    axios.post('https://me-dev.test.nasledie.digital/wp-json/ndriver/v1/river/add',
              riverForm,
              {
                headers: {
                  "Content-Type": "multipart/form-data"
                }
              }
              )
      .then( resp => {
        console.log( 'DATA' );
        console.log( resp.data );
        props.posts.push( resp.data );
        console.log( props.posts );

        // props.posts.push({
        //   'ID': '0000',
        //   'image': {
        //     'large': '',
        //   },
        //   'post_type': "river",
        //   'postmeta': {
        //     'creation_date': ['1706193351'],
        //     'history': ['0'],
        //     'photo_text': [''],
        //     'photo_text_below': [''],
        //     'position':['1'],
        //     'year': ['1107'],
        //   }
        // })
      })
      .catch( error => {
        // props.posts.push({
        //   'ID': '0000',
        //   'image': {
        //     'large': '',
        //   },
        //   'post_type': "river",
        //   'postmeta': {
        //     'creation_date': ['1706193351'],
        //     'history': ['0'],
        //     'photo_text': [''],
        //     'photo_text_below': [''],
        //     'position':['1'],
        //     'year': ['1107'],
        //   }
        // })
      })

    // console.log( props.posts );

  }


  const submit = () => {

    addRiverPost()
  }

  const previewFilePath = computed( () => {
    if ( riverForm.preview ) {

      return URL.createObjectURL( riverForm.preview )
    }
    return '#';
  })
  const uploadFile = (e) => {
    const [file] = e.target.files
    riverForm.preview = file
  }
  // watch( riverForm, () => {
  //   console.log( riverForm.preview );
  // } )
const quillEditorOptions = {
    placeholder: 'Опишите событие',
}



</script>

<template>
  <div class="river_post_container">
    <form
      @submit.prevent.stop="submit"
      class="river__item river-item"
    >
      <div class="river-item__left">


        <div class="river-item__image-container">
          <div class="river-item__image">
            <img class="img-preview" :src=" !riverForm.preview ? defaultImage : previewFilePath" alt="">
          </div>
        </div>


        <div class="input-wrap">
          <input
            type="checkbox"
            name="river-history"
            id="new-river-history"
            v-model="riverForm.history"
          >
          <label for="new-river-history" class="river-history">Выделить событие</label>
          <!-- <p>Выделить событие</p> -->
        </div>
        <div class="input-wrap">
          <input
            type="text"
            name="river-title"
            id="new-river-year"
            v-model="riverForm.year"
          >
          <label for="new-river-year">Год</label>
        </div>
      </div>
      <div class="river-item__right">
        <div class="river-item__text" contenteditable="true">
          <QuillEditor
            theme="snow"
            :options="quillEditorOptions"
            v-model="riverForm.text" content-type="html"

          />
        </div>
      </div>
      <div class="river-item__bottom">
        <input class="river-item__submit"  type="submit" value="Добавить">
      </div>
    </form>
  </div>
</template>

<style scoped>
  .river-item__image {
    display: flex;
    width: 250px;
    height: 170px;
  }

  .river_post_container {
    display: flex;
    flex-flow: row nowrap;
    margin: 50px 0;
    background: #FFFFFF;
    border: 1px solid #EFEFEF;
    box-shadow: 0px 2px 15px rgb(0 0 0 / 12%);
    border-radius: 12px;

    padding: 14px 55px 15px 14px;
    margin-bottom: 30px;
    transition: opacity ease-out 350ms;
    max-width: 750px;
    width: 100%;
  }
  .river-item {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    width: 100%;
  }

  .river-item__left {
    display: flex;
    flex-direction: column;
    width: 250px;
  }
  .river-item__right {
    display: flex;
    flex: 1;
  }
  .river-item__text {
    width: 100%;
    height: 100%;
    padding: 12px 15px;
    border-radius: 8px;
    border: 1px solid rgba(183, 189, 206, 0.30);
    background: #FFF;
    max-height: 250px;
    overflow-y: auto;

  }



 /* .river-item #new-river-history + label {
    display: inline-block;
    width: 20px;
    height: 20px;
    border: 2px solid rgba(68, 68, 68, 0.5);
    border-radius: 4px;
    background: #fff;
    padding: 0;
    margin: 0;
}
.river-item #new-river-history:checked + label:after, .river-item #new-river-history:checked + label:after {
    content: '';
    position: absolute;
    left: 4px;
    top: 1px;
    width: 7px;
    height: 11px;
    border: solid white;
    border-width: 0 2px 2px 0;
    -webkit-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    transform: rotate(45deg);
} */




  .input-wrap {
    max-width: 400px;
    padding-bottom: 0;
    position: relative;
  }


  .input-wrap input[type='text'] {
      font-weight: 400;
      font-size: 16px;
      line-height: 19px;
      width: 100%;
      padding: 18px 10px 7px 9px;
      margin: 0;
      background: 0 0;
      border: 1px solid rgba(183,189,206,.3);
      border-radius: 8px;
      box-sizing: border-box;
  }
  .input-wrap input[type='text'] + label {
      font-weight: 400;
      font-size: 16px;
      line-height: 19px;
      color: #b7bdce;
      position: absolute;
      top: 14px;
      left: 11px;
      transition: all .3s;
      white-space: nowrap;
      text-overflow: ellipsis;
      overflow: hidden;
      width: 90%;
  }




  .river-item__bottom {
    display: flex;
    justify-content: flex-end;
    width: 100%;
  }
  .river-item__submit {
    display: inline-block;
    box-shadow: none;
    cursor: pointer;
    width: 162px;
    font-family: "Proxima Nova";
    font-weight: 700;
    font-size: 16px;
    color: #fff;
    line-height: 50px;
    height: 50px;
    padding: 0;
    margin: 0;
    background-color: #326e5b;
    border: 1px solid #326e5b;
    border-radius: 100px;
  }
  .river-item__submit:hover, .river-item__submit:active {
      background-color: #2c6050;
      border-color: #b7bdce;
  }

</style>
