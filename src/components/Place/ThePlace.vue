<template>
  <img class="places__img" :src="imgSrc" :alt="altTxt" />
</template>

<script>
export default {
  name: "ThePlace",

  props: {
    imgSrc: {
      type: String,
      default: '../../img/pic-404.png',
    },
    altTxt: {
      type: String,
      default: 'Изображение не найдено',
    },
  },

  data() {
    return {
        //
    };
  },
};
</script>
