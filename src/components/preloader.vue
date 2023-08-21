<template>
    <div class="preloader" v-if="isLoading">
        <img src="@/assets/preloader.gif" alt="Loading..." class="loader-gif">
    </div>
  </template>

<script>
  export default {
    props: {
      isLoading: Boolean,
    },
  };
  </script>

  <style>
  .preloader {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8); /* Arkaplan rengi */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loader-gif {
  width: 100px; /* GIF boyutunu ayarlayabilirsiniz */
  height: auto; /* Genişlik oranına göre yüksekliği otomatik ayarlayın */
}
  </style>
