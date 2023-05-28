<script setup>

import Card from './components/Card.vue';

import cat1 from './assets/cat1.jpg';
import cat2 from './assets/cat2.jpg';
import cat3 from './assets/cat3.jpg';
import cat4 from './assets/cat4.jpg';
import back from './assets/back.jpg';

import { onMounted, ref } from 'vue'

let imgs = ref([])
let cats = ref([])
let remaningCats = ref(0)
let firstCard = ref(null)
let clicked = ref(false)
let turns = ref(0)

imgs.value = [
  cat1,
  cat2,
  cat3,
  cat4];

(() => {
  const backElement = document.createElement('img')
  backElement.setAttribute("src", back)

  imgs.value.forEach(img => {
    const element = document.createElement('img')
    element.setAttribute("src", img)
  });

})()


initialize()

function initialize() {

  turns.value = 0;
  firstCard.value = null;

  cats.value = [];

  imgs.value.forEach(img => {
    cats.value.push(createCat(cats.value.length, img))
    cats.value.push(createCat(cats.value.length, img))
  });

  cats.value.sort((a, b) => 0.5 - Math.random());

  remaningCats.value = imgs.value.length;

}
function createCat(id, img) {
  return {
    "id": id,
    "img": img,
    "display": false
  }
}
const reveal = (cat) => {

  if (cat.display || clicked.value)
    return

  clicked.value = true;

  cat.display = true;

  if (firstCard.value == null) {
    firstCard.value = cat
    clicked.value = false
  }
  else {
    turns.value++
    if (firstCard.value.img == cat.img) {
      remaningCats.value--;
      firstCard.value = null;
      clicked.value = false
    }
    else {
      setTimeout(() => {
        firstCard.value.display = false;
        cat.display = false;
        firstCard.value = null;

        clicked.value = false
      }, 500)
    }
  }

}

</script>

<template>
  <header>
    <h1>Cat memory game!</h1>
    <h3 v-if="remaningCats > 0">Turns cout: {{ turns }}</h3>
    <h3 v-else>You won in {{ turns }} turns!</h3>
    <p><button :class="remaningCats > 0 ? 'hidden' : 'restart'" @click="remaningCats > 0 ? null : initialize()">Try
        again</button></p>
  </header>
  <div class="cards">

    <Card v-for="cat in cats" :key="cat.id" :img="cat.display ? cat.img : back" @click="reveal(cat)" class="card"
      :class="[cat.display ? 'revealed' : null]" />
  </div>
</template>

<style scoped>
.cards {
  aspect-ratio: 2/1;
  display: flex;
  align-content: space-around;
  justify-content: space-evenly;
  flex-wrap: wrap;
}

header {
  text-align: center;
  padding: 0;
  margin: 0;
}

header h1 {
  font-size: 3em;
}

header h3 {
  font-size: 1.5em;
  margin-top: 5px;
}

.card {
  opacity: .75;
  cursor: pointer;
  border: .1em solid rgba(255, 255, 255, 0.75);
  border-radius: 20px;
  transition: all 0.15s;

  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.card:hover,
.card:focus,
.revealed {
  opacity: .9;
  border-color: white;
}

.restart {
  color: white;
  opacity: .6;
  background: none;
  border: none;
  outline: none;
  text-decoration: underline;
  cursor: pointer;
  transition: color opacity 0.15s;

  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.restart:hover,
.restart:focus {
  opacity: .9;
  color: rgb(255, 245, 190);
}

.hidden {
  opacity: 0;
  cursor: default;
}

@media (max-width: 800px) {
  .cards {
    width: 100%;
    aspect-ratio: 2 / 3.5;
  }

  header h1 {
    font-size: 2.25em;
  }

  header h3 {
    font-size: 1.5em;
  }

}
</style>
