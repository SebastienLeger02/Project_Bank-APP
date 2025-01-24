<template>
  <div class="container">
    <h1 class="text-center mt-5 mb-5">My Bank App</h1>
    <div class="row mb-4">
      <!-- mis tres cajas -->
      <div class="col">
        <div class="card card-body shadow border-0">
          <h4>Ingresos</h4>
          <h2 class="text-success"> {{ sumPositives }} €</h2>
        </div>
      </div>
      <div class="col">
        <div class="card card-body shadow border-0">
          <h4>Gastos</h4>
          <h2 class="text-danger">{{ expenses }} €</h2>
        </div>
      </div>
      <div class="col">
        <div class="card card-body shadow border-0">
          <h4>Balance</h4>
          <h2 :class="[balance>0 ? 'text-success' : 'text-danger']">{{ balance }} €</h2>
        </div>
      </div>
    </div>

    <div class="col-8 offset-2 d-flex mt-4 justify-content-between">
      <input
        class="form-control"
        type="text"
        placeholder="add a new item"
        v-model="name"
      />
      <input
        class="form-control"
        type="number"
        placeholder="amount"
        v-model.number="amount"
      />
      <button class="btn btn-primary" @click="addItem">Add</button>
    </div>

<!-- search field -->
    <input type="text" placeholder="search..." v-model="searchTerm"/>

    <div>
      <ul class="mt-4 list-group">
        <li
          class="list-group-item d-flex justify-content-between align-item-center"
          v-for="item in filteredItems " :key="item"
        >
          <span> {{ item.name }} </span>
          <div>
            <span
              :class="{
                'text-danger': item.amount < 0,
                'text-success': item.amount >= 0,
              }"
            >
              {{ item.amount }} €</span
            >
            <button class="btn btn-danger ms-3" @click="deleteItem(item.id)">
              Delete
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Box from "./components/Box.vue";
export default {
  name: "App",
  components: {
    Box,
  },
  data() {
    return {
      name: "",
      amount: 0,
      nextId: 4,
      items: [
        { id: 1, name: "Food", amount: -30 },
        { id: 2, name: "Groceries", amount: -42 },
        { id: 3, name: "Salary", amount: 130 },
      ],
      searchTerm:""
    };
  },
  methods: {
    addItem() {
      if(this.name!=="" && this.amount!==0) {
        this.items.push({
          id: this.nextId,
          name: this.name,
          amount: this.amount
        })
        this.nextId++;
     } else {
      alert("te has olvidado a añadir un item")
     }

    },
    deleteItem(id) {
      this.items = this.items.filter(item => item.id!==id)
    },
    
  },
  computed: {
    sumPositives() {
      let sum = 0;
      for(let item of this.items) {
        if(item.amount>0) {
          sum+=item.amount
        }
      }
      return sum;
    },
    expenses() {
      const expenses = this.items.filter((tran) => tran.amount < 0);
      const suma = expenses.reduce((acc, tran) => acc + tran.amount, 0);
      return suma
    },
    balance() {
      return this.items.reduce((acc, tran) => acc + tran.amount, 0);
    },
    filteredItems() {
      return this.items.filter(item => item.name.toLowerCase().includes(this.searchTerm.toLowerCase()))
    }
  }
};
</script>

<style scoped></style>
