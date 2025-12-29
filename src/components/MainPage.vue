<template>
  <div id="main-page">
    <img class="wave" id="wave0" src="@/assets/media/wave0.svg" alt="">
    <img class="wave" id="wave1" src="@/assets/media/wave1.svg" alt="">
    <img class="back" src="@/assets/media/back.svg" alt="back" @click="goBack">

    <gduds v-if="compNum === 0" class="comp" @chosen-gdud="chosenGdud"></gduds>
    <gdud v-if="compNum === 1" class="comp" :gdud="gdud"></gdud>
  </div>
</template>

<script>
import Gduds from "@/components/Gduds";
import Gdud from "@/components/Gdud";

export default {
  name: "main-page",
  components: { Gduds, Gdud },
  data() {
    return {
      compNum: 0,
      gdud: ""
    };
  },
  created() {
    // Restore previous state from sessionStorage
    const savedCompNum = sessionStorage.getItem("mainPageCompNum");
    const savedGdud = sessionStorage.getItem("mainPageGdud");

    if (savedCompNum !== null) this.compNum = parseInt(savedCompNum);
    if (savedGdud !== null) this.gdud = savedGdud;
  },
  watch: {
    compNum(newVal) {
      sessionStorage.setItem("mainPageCompNum", newVal);
    },
    gdud(newVal) {
      sessionStorage.setItem("mainPageGdud", newVal);
    }
  },
  methods: {
    goBack() {
      if (this.compNum === 1) {
        // Go back from gdud to gduds
        this.compNum = 0;
      } else {
        // Go back to open-page
        this.$emit("last-page");
      }
    },
    chosenGdud(gdud) {
      this.gdud = gdud;
      this.compNum = 1;
    }
  }
};
</script>


<style scoped>
    #main-page {
        position: relative;
        top: 0;
        right: 0;
        height: 100vh;
        overflow: hidden;
        width: 100vw;
    }
    .comp {
        position: absolute;
        top: 0;
        right: 0;
        z-index: 3;
    }
    .back {
        max-width: 100%;
        position: absolute;
        top: 1.5vh;
        right: 0.5vw;
        height: 8vh;
        cursor: pointer;
        z-index: 5;
    }
    .wave {
        width: 100vw;
        max-height: 100%;
        position: absolute;
        right: 0;
        user-select: none;
        z-index: 1;
    }
    #wave0 {
        top: -5vh;
    }
    #wave1 {
        top: 30vh;
    }
    @media (max-width: 600px) {
        .back {
            top: 1vh;
            right: 1.5vw;
            height: 5vh;
        }
        .wave {
            width: 500vw;
            right: 0vw;
        }
        #wave0 {
            top: -50vh;
        }
        #wave1 {
            top: 10vh;
        }
    }
</style>
