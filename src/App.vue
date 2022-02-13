<template>
  <div id="app">
    <div id="nav">
      <vue-particles
        color="#555"
        :particleOpacity="0.7"
        :particlesNumber="150"
        shapeType="circle"
        :particleSize="4"
        linesColor="#555"
        :linesWidth="1"
        :lineLinked="true"
        :lineOpacity="0.4"
        :linesDistance="150"
        :moveSpeed="2"
        :hoverEffect="true"
        hoverMode="grab"
        :clickEffect="false"
        clickMode="push"
        class="lizi"
        ref="lizi"
      />
      <div class="btn" @click="btnStyle" ref="btn" :style="list"><div class="btnSm" ref="btnSm"></div></div>
    </div>

    <router-view></router-view>
  </div>
</template>
<script>
export default {
  name: "app",
  data(){
    return{
      list:''
    }
  },
  methods: {
    btnStyle() {
      if (this.$refs.lizi.$el.style.backgroundColor == "rgb(19, 19, 19)") {
        this.$refs.lizi.$el.style.backgroundColor = "rgb(240, 240, 240)";
        this.$refs.btn.style.backgroundColor = "#ccc";
        this.$refs.btnSm.style.left = "5px";
      } else {
        this.$refs.lizi.$el.style.backgroundColor = "rgb(19, 19, 19)";
        this.$refs.btn.style.backgroundColor = "blueviolet";
        this.$refs.btnSm.style.left = "45px";
      }
      this.$refs.lizi.$el.style.transition = "all .4s";
      this.$refs.btn.style.transition = "all .4s";
      this.$refs.btnSm.style.transition = "all .4s";
      
    },
  },
};
</script>
<style>
body {
  width: 100%;
  background-color: rgb(240, 240, 240);
  /* background-color: black; */
  margin: 0;
  padding: 0;
}
.lizi {
  position: fixed;
  top: 0;
  width: 100%;
  height: 100vh;
  z-index: -1;
}

.nav {
  z-index: -1;
}

.btn {
  position: absolute;
  top: 88%;
  left: 20%;
  z-index: 10000 !important;
  cursor: pointer;
  width: 80px;
  height: 40px;
  border-radius: 20px;
  text-align: center;
  line-height: 40px;
  background-color: #ccc;
  box-shadow: 0px 7px 10px rgba(0, 0, 0, 0.6);
}

.btnSm {
  position: absolute;
  left: 5px;
  top: 5px;
  content: "";
  width: 30px;
  height: 30px;
  border-radius: 15px;
  background-color: white;
}

</style>
