<template>
  <div id="app">
    <button @click="showExchangeBookModal" class="button">
      Request To Exchange Book
    </button>

    <transition name="fade" appear>
      <div
        class="modal-overlay"
        v-if="showExchangeBook"
        @click="closeModal"
      ></div>
    </transition>

    <transition name="pop" appear>
      <div class="modal scroll" role="dialog" v-if="showExchangeBook">
        <h1>Request To Exchange Book</h1>
        <form @submit.prevent="submitForm" class="form">
          <div class="input-group">
            <label for="name">Name:</label>
            <input
              class="large-input"
              type="text"
              id="name"
              name="name"
              v-model="name"
              required
              minlength="4"
              maxlength="80"
              size="10"
            />
          </div>

          <div class="input-group">
            <label for="contactNumber">Contact Number:</label>
            <input
              class="large-input"
              v-model="contactNumber"
              type="tel"
              pattern="[0-9]{10}"
              required
            />
          </div>

          <div class="input-group">
            <label for="address">Address:</label>
            <input
              class="large-input"
              v-model="address"
              type="text"
              id="name"
              name="name"
              required
              minlength="4"
              maxlength="80"
            />
          </div>

          <div class="input-group">
            <label for="yourBook">Your Book:</label>
            <input
              class="large-input"
              v-model="yourBook"
              type="text"
              id="name"
              name="name"
              required
              minlength="4"
              maxlength="80"
            />
          </div>

          <div class="input-group">
            <label for="reasonToLike"
              >Why do you want to exchange this Book?</label
            >
            <textarea
              class="large-input textarea"
              v-model="reasonToLike"
              maxlength="1000"
            ></textarea>
            <div class="word-limit-text">
              Remaining {{ reasonToLike.length }}/1000 characters
            </div>
          </div>

          <div class="input-group">
            <label for="reasonToLike"
              >About you Book:</label
            >
            <textarea
              class="large-input textarea"
              v-model="aboutYourBook"
              maxlength="1000"
            ></textarea>
            <div class="word-limit-text">
              Remaining {{ aboutYourBook.length }}/1000 characters
            </div>
          </div>

          <div class="button-group">
            <button @click="closeModal" type="button" class="close-button">
              Close
            </button>
            <button @click="submit" type="submit" class="button">Submit</button>
          </div>
        </form>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

export default {
  data() {
    return {
      showExchangeBook: false,
      name: "",
      contactNumber: "",
      address: "",
      reasonToLike: "",
      yourBook: "",
      aboutYourBook:"",
    };
  },
  methods: {
    showExchangeBookModal() {
      this.showExchangeBook = true;
    },
    closeModal() {
      this.showExchangeBook = false;
      this.resetForm();
    },
    async submitForm() {
      try {
        let resultN = await axios.post(
          "http://localhost:3000/exchangeRequest",
          {
            name: this.name,
            contactNumber: this.contactNumber,
            address: this.address,
            reasonToLike: this.reasonToLike,
            yourBook: this.yourBook,
            aboutYourBook: this.aboutYourBook,
          }
        );
        console.warn(resultN);
        if (resultN.status === 201) {
          this.showExchangeBook = false;
          localStorage.setItem("Donate-request", JSON.stringify(resultN.data));
          toast.success("Request sent to exchange book.");
          this.resetForm();
        } else {
          alert("Please fill the required details");
        }
      } catch (error) {
        this.result = "Error: " + error.result.status;
      }
    },
    // validateWordLimit() {
    //   this.remainingLetters = 1000 - this.reasonToLike.length;
    // },
    resetForm() {
      this.name = "";
      this.contactNumber = "";
      this.address = "";
      this.yourBook = "";
      this.reasonToLike = "";
      this.aboutYourBook = "";
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
.button-group {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  width: 100%;
}
.blue-outline {
  border: 2px solid #3498db; /* Blue color */
  border-radius: 5px;
}
.textarea {
  position: relative;
  height: 150px;
  max-height: 400px;
  width: 100%;
  resize: none;
}
.word-limit-text {
  position: relative;
  float: right;
  font-size: 12px;
  margin-right: 20px;
  margin-bottom: 20px;
}
.word-limit {
  font-size: 14px;
  margin-top: 5px;
}
html {
  height: 100%;
  background: #fff;
  color: #000;
  font-size: 62.5%;
}
body {
  min-height: 100%;
  margin: 0;
  display: grid;
  place-items: center;
  font-size: 1.4rem;
}
.close-button {
  border-radius: 8px;
  color: #648880;
  cursor: pointer;
  display: inline-block;
  font-weight: 400;
  letter-spacing: 0.5px;
  margin: 55px;
  margin-top: 0;
  margin-bottom: 0%;
  text-transform: uppercase;
  border: 1px solid #fff;
  padding: 10px 30px;
}
.close-button:hover {
  background: #fff;
  color: #293f50;
}
.button {
  border-radius: 8px;
  color: #213341;
  cursor: pointer;
  display: inline-block;
  font-weight: 400;
  letter-spacing: 0.5px;
  margin: 55px;
  margin-top: 0;
  margin-bottom: 0%;
  text-transform: uppercase;
  border: 1px solid #fff;
  padding: 10px 30px;
}
.button:hover {
  background: #fff;
  color: black;
}
.modal {
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  text-align: center;
  width: 550px;
  height: 620px;
  overflow: overlay;
  padding: 2rem;
  border-radius: 1rem;
  background: #466368;
  background: -webkit-linear-gradient(#648880, #293f50);
  background: -moz-linear-gradient(#648880, #293f50);
  background: linear-gradient(#648880, #293f50);
  z-index: 999;
  transform: none;
}

::-webkit-scrollbar {
  width: 12px;
  height: 12px;
}
::-webkit-scrollbar-track {
  border: 1px solid #c7d6d9;
  border-radius: 10px;
}
::-webkit-scrollbar-thumb {
  background: #c7d6d9;
  border-radius: 10px;
}
::-webkit-scrollbar-thumb:hover {
  background: #c7d6d9;
}
.modal h1 {
  margin: 0 0 1rem;
  margin-bottom: 30px;
  text-decoration: underline;
}
.modal-overlay {
  content: "";
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 998;
  background: #2c3e50;
  opacity: 0.6;
  cursor: pointer;
}
.input-group {
  margin-bottom: 20px;
  padding: 0 40px;
  text-align: left;
  position: relative; /* Add this line */
}

.label {
  color: white; /* Set the label text color to white */
  position: absolute;
  top: 10px;
  left: 10px;
  transition: transform 0.3s, color 0.3s;
  text-shadow: 0 0 3px rgba(0, 0, 0, 0.8);
  padding: 0 5px;
  border-radius: 5px;
  z-index: 1;
  box-shadow: none;
}

.large-input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  margin-top: 5px;
  text-align: left;
  background: #c7d6d9;
  outline: white;
  box-shadow: white;
}

/* Add the following styles for the input transition effect */
.input-group:hover .large-input,
.input-group:focus-within .large-input,
.large-input:not(:placeholder-shown) {
  border: none;
  outline: white;
  box-shadow: white;
}

/* Add the following styles for the label transition effect */
.input-group:hover .label,
.input-group:focus-within .label,
.large-input:not(:placeholder-shown) + .label {
  transform: translateY(-20px) scale(0.8);
  color: #3498db; /* Change label color on focus/hover */
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s linear;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.pop-enter-active,
.pop-leave-active {
  transition: transform 0.4s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.4s linear;
}
.pop-enter,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%);
}
</style>



