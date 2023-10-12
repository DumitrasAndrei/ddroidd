<script setup lang="ts">
import AplicationForm from './AplicationForm.vue'
import { ref } from 'vue'

const showForm = ref(false)

function toggleForm() {
  showForm.value = true
}

function refreshPage(){
  window.location.reload()
}
</script>

<template>
  <section class="home-screen">
    <div class="home-screen__header" >
      <div>
        <img src="../assets/img/ddroidd_logo.svg"  @click="refreshPage"/>
      </div>
      <div>
        <h1 class="home-screen__header-title">
          Autumn - Winter Bootcamp
        </h1>
      </div>
      <div>
        <button class="home-screen__button" @click="toggleForm" v-if="!showForm">
          Join Us
        </button>
      </div>
    </div>
    <div class="home-screen__content">
      <div class="home-screen__content-img-container" v-if="!showForm">
        <img src="../assets/img/destructuring.svg" />
        <img src="../assets/img/WebPage_logo.svg" />
      </div>
      <AplicationForm v-else />
      <div class="home-screen__content-button-container" v-if="!showForm">
        <button class="home-screen__button" @click="toggleForm">
          Join Us
        </button>
      </div>
    </div>
    <div class="home-screen__footer">
      <h2 class="home-screen__footer-text">
        Come to the dark side... we have <img src="../assets/img/cookies.svg" />
      </h2>
    </div>
  </section>
</template>

<style>
.home-screen__header {
  display: flex;
  justify-content: space-between;
  background: var(--prussian-blue);
  align-items: center;
  padding: 0 21px;
}

.home-screen__header-title {
  color: var(--white);
  font-size: 48px;
  font-weight: 400;
  line-height: normal;
}

.home-screen__button {
  background: var(--school-bus-yellow);
  width: 239px;
  height: 47px;
  border-radius: 50px;
  border: none;
}

.home-screen__content {
  background: var(--athens-gray);
  width: 100%;
  max-width: 1374px;
  height: 100%;
  min-height: 851px;
  margin: 0 273px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 132px 12px;
}

.home-screen__content-img-container {
  display: flex;
}

.home-screen__content-button-container {
  display: flex;
  justify-content: center;
}

.home-screen__footer {
  background: var(--dove-gray);
  padding: 30px 0;
}

.home-screen__footer-text {
  color: var(--white);
  font-size: 36px;
  font-weight: 400;
  line-height: normal;
  text-align: center;
}
</style>
