<script setup>
defineProps({
  // msg: {
  //   type: String,
  //   required: true
  // }
})

const defaultImage = 'https://me-dev.test.nasledie.digital/wp-content/themes/nasldnew/assets/img/river-img.png';
const submit = () => {
  console.log('submit');
}
</script>

<template>
  <form
    @submit.prevent.stop="submit"
  >
      <img :src="defaultImage" alt="">
      <div class="input-label">
        <label for="new-river-year">Год</label>
        <input type="text" name="river-title" id="new-river-year">
      </div>
      <div class="input-label">
        <label for="new-river-history">Выделить событие</label>
        <input type="checkbox" name="river-history" id="new-river-history">
      </div>
      <div class="input-label">
        <label for="new-river-text">Текс</label>
        <textarea type="text" name="river-text" id="new-river-text"></textarea>
      </div>
      <input type="submit" value="Добавить">
    </form>
</template>

<style scoped>

</style>
