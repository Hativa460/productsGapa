<template>
  <div id="gdud">
    <div class="title">
      תוצרי הדרכה - {{ gdud }}
    </div>

    <div
      v-if="products && products.length"
      class="products-container"
    >
      <div class="products-wrapper">

        <div
          class="product"
          v-for="(product,index) in products"
          :key="index"
        >
          <a
            :href="product.link"
            class="product-content"
          >

            <img
              class="pic"
              :src="require(`@/assets/media/${product.icon}.svg`)"
              alt=""
            >

            <div class="name">
              {{ product.name }}
            </div>

          </a>
        </div>

      </div>
    </div>

    <div
      v-else
      class="appology"
    >
      לצערנו עוד לא פיתחנו תוצרי הדרכה לגדוד שלך.
      <br>
      אתה מוזמן לפנות אלינו עבור פיתוח לומדות וסרטונים ונשמח לעזור.
    </div>

  </div>
</template>

<script>
import json from "../../text.json";

export default {

  name: "gdud",

  props: {
    gdud: {
      type: [String, Number],
      required: true
    }
  },

  data() {
    return {
      products: []
    };
  },

  created() {
    this.products = json[this.gdud] || [];
  },

  watch: {

    gdud(newVal) {
      this.products = json[newVal] || [];
    }

  }

};
</script>

<style scoped>

#gdud{
    width:100vw;
    height:100vh;
    overflow-y:auto;
    overflow-x:hidden;
    box-sizing:border-box;
}

.title{
    font-family:"assistant-extraBold";
    width:90%;
    margin:auto;
    margin-top:5vh;
    margin-bottom:4vh;
    text-align:center;
    font-size:3vw;
    text-shadow:1px 0px 3px #000;
}

.appology{
    width:80%;
    margin:auto;
    margin-top:10vh;
    text-align:center;
    font-size:1.7vw;
}

a{
    text-decoration:none;
    color:#e8e8e8;
}

.products-container{
    width:100%;
    display:flex;
    justify-content:center;
    padding-bottom:5vh;
}

.products-wrapper{
    width:92%;
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:2vw;
}

.product{

    height:40vh;

    border-radius:1vw;

    overflow:hidden;

}

.product-content{

    width:100%;
    height:100%;

    display:flex;
    flex-direction:column;

    justify-content:space-between;

    align-items:center;

    background:linear-gradient(180deg,#e8e8e834,#e8e8e89f);

    border:.25vh solid #75c2e6;

    border-radius:inherit;

    box-sizing:border-box;

    padding:1vw;

    transition:.25s;

}

.product-content:hover{

    transform:scale(1.03);

}

.pic{

    max-width:75%;

    max-height:16vh;

    margin-top:3vh;

}

.name{

    font-size:1.55vw;

    text-align:center;

    margin-bottom:2vh;

    width:90%;

}


/* MOBILE */

@media(max-width:600px){

#gdud{

    overflow-y:auto;

}

.title{

    font-size:7vw;

    margin-top:3vh;

    margin-bottom:3vh;

}

.products-wrapper{

    width:94%;

    grid-template-columns:repeat(2,1fr);

    gap:4vw;

}

.product{

    height:28vh;

    border-radius:3vw;

}

.pic{

    max-width:75%;

    max-height:9vh;

    margin-top:7vh;

}

.name{

    font-size:4vw;

    margin-bottom:2vh;

}

.appology{

    font-size:5vw;

    width:90%;

}

}

</style>