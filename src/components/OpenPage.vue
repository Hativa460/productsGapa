<template>
  <div id="open-page" 
       @mousemove="updateShadow" 
       @touchmove="updateShadow" 
       @touchstart="pauseAutoColor" 
       @touchend="resumeAutoColor">
    <img class="wave" id="wave0" src="@/assets/media/wave0.svg" alt="">
    <img class="wave" id="wave1" src="@/assets/media/wave1.svg" alt="">
    <div class="title-container">
      <div class="small-title">צוות פל"א 460</div>
      <div class="main-title">עזרי למידה והדרכה</div>
      <button @click="nextPage">שנתחיל?</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "open-page",
  data() {
    return {
      autoColorInterval: null,
      isMobile: false,
      hue: 0,
    };
  },
  mounted() {
    this.isMobile = window.innerWidth <= 600;
    if (this.isMobile) {
      this.startAutoColor();
    }
  },
  beforeUnmount() {
    this.clearAutoColor();
  },
  methods: {
    nextPage() {
      this.$emit("next-page");
    },
    startAutoColor() {
      this.clearAutoColor();
      this.autoColorInterval = setInterval(() => {
        this.hue = (this.hue + 2) % 360;
        const color = `hsl(${this.hue}, 100%, 50%)`;
        document.documentElement.style.setProperty("--shadow-color", color);
      }, 100); // smooth color cycle
    },
    clearAutoColor() {
      if (this.autoColorInterval) {
        clearInterval(this.autoColorInterval);
        this.autoColorInterval = null;
      }
    },
    pauseAutoColor() {
      this.clearAutoColor();
    },
    resumeAutoColor() {
      if (this.isMobile) this.startAutoColor();
    },
    updateShadow(e) {
      const x = e.touches ? e.touches[0].clientX : e.clientX;
      const y = e.touches ? e.touches[0].clientY : e.clientY;
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 2;

      const offsetX = (x - centerX) / 8;
      const offsetY = (y - centerY) / 8;

      const hue = (x / window.innerWidth) * 360;
      const color = `hsl(${hue}, 100%, 50%)`;

      document.documentElement.style.setProperty("--shadow-x", `${offsetX}px`);
      document.documentElement.style.setProperty("--shadow-y", `${offsetY}px`);

      if (!this.isMobile) {
        // Desktop → fully interactive
        document.documentElement.style.setProperty("--shadow-color", color);
      } else {
        // Mobile → override auto cycle only while finger is down
        if (!this.autoColorInterval) {
          document.documentElement.style.setProperty("--shadow-color", color);
        }
      }
    },
  },
};
</script>

<style scoped>
#open-page {
  position: relative;
  top: 0;
  right: 0;
  height: 100vh;
  overflow: hidden;
  width: 100vw;
}

/* Waves glow follows mouse/touch */
.wave {
  width: 100vw;
  max-height: 100%;
  position: absolute;
  right: 0;
  user-select: none;
  z-index: 1;
  transition: filter 0.05s linear, transform 0.1s ease-out;
  filter: drop-shadow(var(--shadow-x, 0px) var(--shadow-y, 0px) 5vh var(--shadow-color, #75C2E6));
  transform: scale(1.03);
}

#wave0 {
  top: -5vh;
}
#wave1 {
  top: 30vh;
}

.title-container {
  position: absolute;
  top: 50%;
  right: 50%;
  z-index: 2;
  transform: translate(50%, -50%);
}

.small-title {
  font-family: "assistant-light";
  font-size: 2.25vw;
  user-select: none;
}

.main-title {
  font-family: "assistant-extrabold";
  position: relative;
  margin-top: 0vh;
  font-size: 5.25vw;
  user-select: none;
  text-shadow: 2px 2px 20px rgba(0, 0, 0, 0.9);
}

button {
  padding: 1vw;
  border: 0.2vh solid #75C2E6;
  border-radius: 2vw;
  color: #E8E8E8;
  z-index: 3;
  cursor: pointer;
  background: linear-gradient(180deg, #e8e8e81f, #e8e8e85f);
  position: relative;
  font-size: 1.5vw;
  -webkit-box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
  box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
  transition: all 150ms;
  overflow: hidden;
  font-family: "assistant";
  margin-bottom: 10vh;
}

button::before {
  content: "";
  position: absolute;
  top: 0;
  right: 0;
  height: 100%;
  width: 0;
  border-radius: 2vw;
  background-color: #75C2E6;
  z-index: -1;
  -webkit-box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
  box-shadow: 4px 8px 19px -3px rgba(0,0,0,0.27);
  transition: all 150ms
}

button:hover {
  color: #1F1E23;
  border-color: #00000000;
}

button:hover::before {
  width: 100%;
}

@media (max-width: 600px) {
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
  .small-title {
    font-size: 6vw;
  }
  .main-title {
    font-size: 10vw;
    width: 80vw;
    margin: auto;
    margin-bottom: 2vh;
  }
  button {
    margin-bottom: 20vh;
    font-size: 5vw;
    border-radius: 5vw;
    padding: 2vw;
  }
  button::before {
    border-radius: 5vw;
  }
}
</style>
