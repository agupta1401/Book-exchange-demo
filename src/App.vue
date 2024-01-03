<template>
  <div id="app">
    <div class="showcase">
      <img src="./assets/bookbg.jpg" alt="Picture" />
      <div class="overlay">
        <h2>Welcome to Book Donate/Exchange Platform</h2>
        <br>
        <BuyDonatedBookButton @click="showModal('buyDonatedBook')" />
        <br>
        <ExchangeBookButton @click="showModal('exchangeBook')" />
      </div>
    </div>
  </div>
</template>

<script>
import BuyDonatedBookButton from "./components/donateRequest.vue";
import ExchangeBookButton from "./components/exchangeRequest.vue";

export default {
  name: "App",
  components: {
    BuyDonatedBookButton,
    ExchangeBookButton,
  },
  data() {
    return {
      showModalType: null,
    };
  },
  methods: {
    showModal(type) {
      this.showModalType = type;
    },
    hideModal() {
      this.showModalType = null;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

body {
  font-family: "Open Sans";
  overflow-x: hidden;
}

.showcase {
  color: white;
  text-align: center;
}

.showcase img {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index:99
}

.showcase .overlay {
  width: 100%;
  height: 500px;
  position: absolute;
  top: 0;
  left: 0;
  z-index:999
}

.showcase h2 {
  margin-top: 170px;
  margin-bottom: 40px;
  font-size: 3em;
  font-family: 'Inter', sans-serif;
}

</style>
