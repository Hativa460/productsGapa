<template>
  <div id="app">
    <div class="logo-container">
      <img src="@/assets/media/460.svg" alt="460" class="logo">
      <img src="@/assets/media/pele.png" alt="logo" class="logo">
    </div>
    
    <open-page v-if="page === 0" @next-page="togglePage"></open-page>
    <main-page v-if="page === 1" @last-page="togglePage"></main-page>
  </div>
</template>

<script>
import OpenPage from "@/components/OpenPage";
import MainPage from "@/components/MainPage";

export default {
  name: "app",
  data() {
    return {
      page: 0
    };
  },
  created() {
    // Restore page from sessionStorage
    const savedPage = sessionStorage.getItem("appPage");
    if (savedPage !== null) this.page = parseInt(savedPage);
  },
  watch: {
    page(newVal) {
      sessionStorage.setItem("appPage", newVal);
    }
  },
  components: {
    OpenPage,
    MainPage
  },
  methods: {
    togglePage() {
      this.page = this.page === 0 ? 1 : 0;
    }
  }
};
</script>



<style>
  @font-face {
    font-family: "assistant-light";
    src: url(assets/fonts/Assistant-Light.ttf);
  }
  @font-face {
    font-family: "assistant-bold";
    src: url(assets/fonts/Assistant-Bold.ttf);
  }
  @font-face {
    font-family: "assistant";
    src: url(assets/fonts/Assistant-Regular.ttf);
  }
  @font-face {
    font-family: "assistant-extrabold";
    src: url(assets/fonts/Assistant-ExtraBold.otf);
  }
html, body, #app {
  width: 100vw;
  height: 100vh;
  margin: 0;
  overflow: hidden;
  padding: 0;
}

body {
  font-family: "assistant";
  text-align: center;
  direction: rtl;
  color: #E8E8E8;
  background: linear-gradient(135deg, #5E81AC, #475C7A, #364256, #2B2A30, #1F1E23, #1F1E23);
}
.logo-container {
    position: absolute;
    display: flex;
    flex-direction: row;
    direction: ltr;
    top: 1.5vh;
    left: 0.5vw;
    z-index: 5;
}
  .logo {
    max-width: 100%;  
    height: 8vh;
    margin: 1%;
  }

  @media (max-width: 600px) {
    .logo-container {
      top: 1vh;
      left: 1.5vw;
    }
 .logo {
    height: 5vh;
  }
}
</style>