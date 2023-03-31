<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import Greet from "./components/Greet.vue";
import { ref, computed } from "vue";

const header = ref("Shopping List App")

const editing = ref(false)
const items = ref([
  {id: 1, label: "10 Party Hats", purchased: true, hiPri: true},
  {id: 2, label:"2 board games", purchased: false, hiPri: false},
  {id: 3, label:"20 cups", purchased: true, hiPri: false},
  {id: 4, label:"1 Awesome Course", purchased: false, hiPri:true}
])
const reversedItems = computed(() => {
  return [...items.value].reverse()//reversed array
})

const newItem = ref("")
const newItemHiPri = ref(false)
const saveItem = () => {
  items.value.push(
    {
      id: items.value.length + 1,
      label: newItem.value,
      hiPri: newItemHiPri.value
    })
  newItem.value = ""
  newItemHiPri.value = ""
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">
      Cancel
    </button>
    <button v-else="!editing" class="btn" @click="doEdit(true)">
      Add Item
    </button>
  </div>

  <form 
    class="add-item-form"
    v-if="editing"
    @submit.prevent ="saveItem"
  >
    <input
      v-model="newItem"
      type="text"
      placeholder="Add an Item"
    >
    <label>
      <input type="checkbox" v-model="newItemHiPri">
      High Priority
    </label>
    <button 
      :disabled="newItem.length === 0"
      class="btn btn-primary">
      Save Item
    </button>
      
  </form>

  <ul>
    <li 
      v-for="(item, index) in reversedItems"
      :key="item.id"
      :class="{strikeout: item.purchased, priority: item.hiPri}"
      class="static-class"
      @click="togglePurchased(item)"
      >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">
    Nothing to see here
  </p>
  <!-- <div class="container">
    <h1>Welcome to Tauri!</h1>

    <div class="row">
      <a href="https://vitejs.dev" target="_blank">
        <img src="/vite.svg" class="logo vite" alt="Vite logo" />
      </a>
      <a href="https://tauri.app" target="_blank">
        <img src="/tauri.svg" class="logo tauri" alt="Tauri logo" />
      </a>
      <a href="https://vuejs.org/" target="_blank">
        <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
      </a>
    </div>

    <p>Click on the Tauri, Vite, and Vue logos to learn more.</p>

    <p>
      Recommended IDE setup:
      <a href="https://code.visualstudio.com/" target="_blank">VS Code</a>
      +
      <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
      +
      <a href="https://github.com/tauri-apps/tauri-vscode" target="_blank"
        >Tauri</a
      >
      +
      <a href="https://github.com/rust-lang/rust-analyzer" target="_blank"
        >rust-analyzer</a
      >
    </p>

    <Greet />
  </div> -->
</template>

<style scoped>
.logo.vite:hover {
  filter: drop-shadow(0 0 2em #747bff);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #249b73);
}
</style>
