<template>
  <div id="burger-table">
    <div>
      <div id="burger-table-heading">
        <div id="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <ul>
            <li v-for="(optional, index) in burger.opcionais" :key="index">{{ optional }}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status">
            <option value="">Selecione</option>
            <option
              v-for="s in status"
              :key="s.id"
              :value="s.tipo"
              :selected="burger.status === s.tipo"
              >{{ s.tipo }}</option
            >
          </select>
          <button class="delete-btn">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
    };
  },
  methods: {
    async getOrders() {
      const req = await fetch('http://localhost:3000/burgers');
      const res = await req.json();

      this.burgers = res;
    },
    async getStatus() {
      const req = await fetch('http://localhost:3000/status');
      const res = await req.json();

      this.status = res;
    },
  },
  mounted() {
    this.getOrders();
    this.getStatus();
  },
};
</script>
<style lang="scss" scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;

  div {
    width: 19%;
  }

  #order-id {
    width: 5%;
  }
}
.burger-table-row {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  div {
    width: 19%;
  }

  .order-number {
    width: 5%;
  }
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;

  &:hover {
    background-color: transparent;
    color: #222;
  }
}
</style>
