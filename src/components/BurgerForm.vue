<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="burger-form">
        <div class="input-container">
          <label for="name">Nome do cliente</label>
          <input type="text" id="name" name="name" v-model="name" placeholder="Digite seu nome" />
        </div>
        <div class="input-container">
          <label for="bread">Escolha o pão</label>
          <select name="bread" id="bread" v-model="bread">
            <option value="">Selecione seu pão</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{
              bread.tipo
            }}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="steak">Escolha a carne</label>
          <select name="steak" id="steak" v-model="steak">
            <option value="">Selecione sua carne</option>
            <option v-for="steak in steaks" :key="steak.id" :value="steak.tipo">{{
              steak.tipo
            }}</option>
          </select>
        </div>
        <div id="optional-container" class="input-container">
          <label for="optional">Selecione os opcionais</label>
          <div class="checkbox-container" v-for="optional in optionalsData" :key="optional.id">
            <input type="checkbox" name="optional" v-model="optionals" :value="optional.tipo" />
            <span>{{ optional.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input @click="createBurger" type="button" class="submit-btn" value="Criar meu Burger" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue';

export default {
  name: 'BurgerForm',
  components: {
    Message,
  },
  data() {
    return {
      bread: null,
      steak: null,
      optionalsData: null,
      name: null,
      breads: null,
      steaks: null,
      optionals: [],
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch('http://localhost:3000/ingredientes');
      const data = await req.json();

      this.breads = data.paes;
      this.steaks = data.carnes;
      this.optionalsData = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();
      const data = {
        nome: this.name,
        carne: this.steak,
        pao: this.bread,
        opcionais: Array.from(this.optionals),
        status: 'Solicitado',
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch('http://localhost:3000/burgers', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido N°${res.id} realizado com sucesso`;

      setTimeout(() => {
        this.msg = null;
      }, 3000);

      this.name = null;
      this.steak = null;
      this.bread = null;
      this.optionals = null;
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>
<style lang="scss" scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input,
  select {
    padding: 5px 10px;
    width: 300px;
  }

  #optional-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #optional-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: center;
    width: 50%;
  }

  .checkbox-container {
    span {
      margin-left: 6px;
      font-weight: bold;
    }

    input {
      width: auto;
    }
  }

  .submit-btn {
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
}
</style>
