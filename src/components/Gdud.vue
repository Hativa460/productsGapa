<template>
  <div id="gdud">
    <div class="title">תוצרי הדרכה - {{ gdud }}</div>

    <div v-if="products && products.length > 3" class="products-container">
      <!-- Right arrow visually (‹) stays on the right -->
      <div class="arrow right-arrow" @click="onRightArrow">&#8249;</div>

      <div class="products-wrapper">
        <div
          v-for="(product, index) in animatedProducts"
          :key="product.key"
          class="product"
          :style="product.style"
        >
          <a :href="product.link" class="product-content">
            <img
              class="pic"
              :src="require(`@/assets/media/${product.icon}.svg`) "
              alt=""
            />
            <div class="name">{{ product.name }}</div>
          </a>
        </div>
      </div>

      <!-- Left arrow visually (›) stays on the left -->
      <div class="arrow left-arrow" @click="onLeftArrow">&#8250;</div>
    </div>

    <div v-else>
      <div v-if="!products || products.length === 0" class="appology">
        לצערנו עוד לא פיתחנו תוצרי הדרכה לגדוד שלך. אתה מוזמן לפנות אלינו
        עבור פיתוח לומדות וסרטונים ונשמח לעזור.
      </div>
      <div v-else class="products-container">
        <div
          class="product"
          v-for="(product, index) in products"
          :key="index"
        >
          <a :href="product.link" class="product-content">
            <img
              class="pic"
              :src="require(`@/assets/media/${product.icon}.svg`) "
              alt=""
            />
            <div class="name">{{ product.name }}</div>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import json from "../../text.json";

export default {
  name: "gdud",
  props: {
    gdud: { type: [String, Number], required: true }
  },
  data() {
    return {
      products: json[this.gdud] || [],
      startIndex: 0,
      isAnimating: false,
      animatedProducts: [],
      uniqueId: 0 // counter for unique keys
    };
  },
  computed: {
    visibleProducts() {
      if (!this.products || this.products.length === 0) return [];
      const len = this.products.length;
      const endIndex = this.startIndex + 3;
      if (endIndex <= len) return this.products.slice(this.startIndex, endIndex);
      return [
        ...this.products.slice(this.startIndex, len),
        ...this.products.slice(0, endIndex - len)
      ];
    }
  },
  mounted() {
    this.resetAnimatedProducts();
  },
  methods: {
    nextKey(p, extra = "") {
      this.uniqueId++;
      return `${p.icon}-${this.uniqueId}${extra}`;
    },
    resetAnimatedProducts() {
      if (!this.visibleProducts || this.visibleProducts.length === 0) {
        this.animatedProducts = [];
        return;
      }
      this.animatedProducts = this.visibleProducts.map(p => ({
        ...p,
        key: this.nextKey(p),
        style: { transform: "translateX(0%)", transition: "", opacity: 1 }
      }));
    },
    onRightArrow() {
      if (this.isAnimating) return;
      this.isAnimating = true;

      const nextIndex = (this.startIndex + 1) % this.products.length;
      const newCard = this.products[(this.startIndex + 3) % this.products.length];

      // Fade out leftmost card very fast
      if (this.animatedProducts.length > 0) {
        this.animatedProducts[0].style = {
          ...this.animatedProducts[0].style,
          opacity: 0,
          transition: "opacity 0.08s ease-out"
        };
      }

      // Slide remaining cards right
      for (let i = 1; i < this.animatedProducts.length; i++) {
        this.animatedProducts[i].style = {
          ...this.animatedProducts[i].style,
          transform: "translateX(105%)",
          transition: "transform 0.3s ease-in-out"
        };
      }

      // Add new card at the end sliding in with fade-in
      this.animatedProducts.push({
        ...newCard,
        key: this.nextKey(newCard, "-new"),
        style: {
          transform: "translateX(105%)",
          transition: "transform 0.3s ease-in-out, opacity 0.3s ease-in",
          opacity: 0
        }
      });

      this.$nextTick(() => {
        const lastCard = this.animatedProducts[this.animatedProducts.length - 1];
        lastCard.style.opacity = 1;
      });

      setTimeout(() => {
        this.startIndex = nextIndex;
        this.resetAnimatedProducts();
        this.isAnimating = false;
      }, 300);
    },
    onLeftArrow() {
      if (this.isAnimating) return;
      this.isAnimating = true;

      const prevIndex =
        (this.startIndex - 1 + this.products.length) % this.products.length;
      const newCard = this.products[prevIndex];

      // Fade out rightmost card very fast
      if (this.animatedProducts.length > 0) {
        const lastIndex = this.animatedProducts.length - 1;
        this.animatedProducts[lastIndex].style = {
          ...this.animatedProducts[lastIndex].style,
          opacity: 0,
          transition: "opacity 0.08s ease-out"
        };
      }

      // Slide remaining cards left
      for (let i = 0; i < this.animatedProducts.length - 1; i++) {
        this.animatedProducts[i].style = {
          ...this.animatedProducts[i].style,
          transform: "translateX(-105%)",
          transition: "transform 0.3s ease-in-out"
        };
      }

      // Add new card at start sliding in with fade-in
      this.animatedProducts.unshift({
        ...newCard,
        key: this.nextKey(newCard, "-new"),
        style: {
          transform: "translateX(-105%)",
          transition: "transform 0.3s ease-in-out, opacity 0.3s ease-in",
          opacity: 0
        }
      });

      this.$nextTick(() => {
        const firstCard = this.animatedProducts[0];
        firstCard.style.opacity = 1;
      });

      setTimeout(() => {
        this.startIndex = prevIndex;
        this.resetAnimatedProducts();
        this.isAnimating = false;
      }, 300);
    }
  },
  watch: {
    gdud(newVal) {
      this.products = json[newVal] || [];
      this.startIndex = 0;
      this.resetAnimatedProducts();
    }
  }
};
</script>

<style scoped>
#gdud {
  height: 100vh;
  overflow: hidden;
  width: 100vw;
}

.title {
  font-family: "assistant-extraBold";
  margin: auto;
  width: 80vw;
  margin-top: 7vh;
  font-size: 3.25vw;
  text-shadow: 1px 0px 3px #000000;
}

.appology {
  font-size: 1.75vw;
  margin: auto;
  margin-top: 2vh;
  width: 60vw;
}

a {
  text-decoration: none;
  color: #e8e8e8;
}

.products-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 95vw;
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, -50%);
}

.products-wrapper {
  display: flex;
  position: relative;
}

.arrow {
  font-size: 10vw;
  cursor: pointer;
  user-select: none;
  padding: 0 1vw;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 10;
}

/* Fix arrow positions to match original design */
.left-arrow {
  left: 5%;
}

.right-arrow {
  right: 5%;
}

.product {
  height: 40vh;
  width: 25vw;
  margin: 3%;
  border-radius: 1vw;
  cursor: pointer;
  box-sizing: border-box;
  position: relative;
}

.product-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  width: 100%;
  text-align: center;
  background: linear-gradient(180deg, #e8e8e834, #e8e8e89f);
  border: 0.25vh solid #75c2e6;
  border-radius: inherit;
  padding: 2%;
  box-sizing: border-box;
}

.pic {
  max-height: 15vh;
  max-width: 100%;
  margin-top: 3vh;
}

.name {
  font-size: 1.75vw;
}

/* MOBILE */
@media (max-width: 600px) {
  .title {
    margin-top: 7vh;
    font-size: 7vw;
  }

  .appology {
    font-size: 5vw;
    width: 80vw;
  }

  .product {
    width: 27vw;
    height: 22vh;
    border-radius: 3vw;
  }

  .product-content {
    padding: 1%;
  }

  .pic {
    height: 6vh;
  }

  .name {
    font-size: 4.25vw;
  }

  .arrow {
    font-size: 15vw;
    padding: 0px;
  }
  .left-arrow {
    left: 2%;
  }
  .right-arrow {
    right: 2%;
  }
}
</style>
