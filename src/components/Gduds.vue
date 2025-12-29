<template>
    <div id="gduds">
        <div class="title">תוצרי הדרכה</div>
        <div class="big-container">
            <div 
              v-for="(row, rowIndex) in gdudRows" 
              :key="rowIndex" 
              class="inner-container animate__animated"
              :class="rowIndex % 2 === 0 ? 'animate__backInRight' : 'animate__backInLeft'"
            >
                <div 
                  v-for="gdud in row" 
                  :key="gdud" 
                  class="gdud" 
                  @click="pickGdud(gdud)"
                >
                  {{ gdud }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "gduds",
    data() {
        return {
            gdudRows: [
                [195, 532, 196],
                [198, 183, 460]
            ]
        };
    },
    methods: {
        pickGdud(gdud) {
            this.$emit("chosen-gdud", gdud);
        }
    }
}
</script>

<style scoped>
#gduds {
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
.big-container {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    position: absolute;
    top: 50%;
    right: 50%;
    transform: translate(50%, -50%);
}
.inner-container {
    display: flex;
    flex-wrap: nowrap;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}
.gdud {
    background: linear-gradient(180deg, #e8e8e834, #e8e8e89f);
    border: 0.25vh solid #75C2E6;
    margin: 3%;
    font-family: "assistant-extrabold";
    border-radius: 1.25vw;
    font-size: 6.5vw;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    cursor: pointer;
    padding: 1% 10%;
    transition: all 0.3s;
}
.gdud:hover {
    scale: 0.9;
}
@media (max-width: 600px) {
  .title {
      margin-top: 7vh;
      font-size: 7vw;
  }   
  .big-container {
    flex-direction: row;
    height: 100%;
    justify-content: center;
  }
  .inner-container {
    flex-direction: column;
    justify-content: space-evenly;
    height: 40%;
  }
  .gdud {
    font-size: 17vw;
    border-radius: 2.5vw;
    margin-left: 1vw;
    margin-top: 1vh;
    margin-bottom: 1vh;
    margin-right: 1vw;
    padding: 0.5vh 6vw;
  }
}
</style>
