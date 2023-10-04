<script setup lang="ts">
import { ref } from "vue";
import Sortable from "./components/Sortable.vue";

// use component
import { createApp } from 'vue';
const app = createApp({})
app.component('Sortable', Sortable)

let options = {
  animation: 150
}

let items = ref([
  { id: 1, title: "John Martson" },
  { id: 2, title: "Arthur Morgan" },
])

const add = () => {
  const names = [
    "Alice",
    "Bob",
    "Charlie",
    "David",
    "Eva",
    "Frank",
    "Grace",
    "Helen",
    "Ivy",
    "Jack",
    "Kara",
    "Leo",
    "Mara",
    "Nina",
    "Oscar"
  ];

  // create random
  let title = names[Math.floor(Math.random() * names.length)];

  // prevent duplicate
  let attempts = 0;  // Track the number of tries

  while (items.value.find(item => item.title === title) && attempts < names.length) {
    title = names[Math.floor(Math.random() * names.length)];
    attempts++;
  }

  // If after all attempts, we still have a duplicate, we know all names are used
  if (attempts !== names.length) {
    items.value.push({
      id: new Date().getTime(),
      title: title
    });
  } else {
    console.log("All names are already used.");
  }
}

const sortByName = () => {
  items.value.sort((a, b) => a.title.localeCompare(b.title))
}

const remove = (id: any) => {
  items.value = items.value.filter(item => item.id !== id)
}

</script>

<template>
  <div>
    <button class="btn btn-primary" @click="add">Add</button>
    <button class="btn btn-primary" @click="sortByName">Sort By Name</button>

    <Sortable v-model="items" item-key="list" :options="options" tag="ul">
      <template #item="{ item, i }">
        <li class="list-group-item cursor-pointer" :key="item.id">
          {{ item.title }} <a href="#" @click="remove(item.id)">Remove</a>
        </li>
      </template>

    </Sortable>
  </div>
</template>


