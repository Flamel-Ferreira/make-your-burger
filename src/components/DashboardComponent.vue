<template>
  <div id="burger_table">
    <!-- Pop up de Feedback -->
    <MessageComponent :msg="msg" v-show="msg"/>
    <div>
      <!-- Cabeçalho da Tabela -->
      <div id="burger_table_heading">
        <div class="order_id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>

    <!-- Linhas da Tabela -->
    <div id="burger_table_rows">
      <!-- Linha única da Tabela (onde terá as informações do Burger) -->
      <div class="burger_table_row" v-for="burger in burgers" :key="burger.id">
        <div class="order_number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>

        <div>
          <ul>
            <li v-for="(opcional,index) in burger.opcionais" :key="index">{{ opcional }}</li>
          </ul>
        </div>

        <select name="status" class="status" @change="updatedBurger($event,burger.id)">
          <option value="">== Selecione ==</option>
          <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{ s.tipo }}</option>
        </select>

        <button class="delete_btn" @click="deleteBurger(burger.id)">Cancelar</button>
        
      </div>

    </div>
  </div>
</template>

<script>
import MessageComponent from "./MessageComponent.vue"
export default {
  components:{
    MessageComponent
  },
  data(){
    return{
      burgers: null,
      burger_id: null,
      status:[],
      msg: null
    }
  },
  methods:{
    // Requisição para o fake backend sobre os Burgers
    async getPedidos(){
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();

      // Armazenando os burgers vindos do servidor
      this.burgers = data;
      
      //resgatar os status
      this.getStatus();

    },
    // Requisição para o fake backend sobre o status dos Burguers
    async getStatus(){
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      // Armazenando os status dos Burgers
      this.status = data;
    },
    // Delentando o burguer quando clicar em cancelar, pede id para saber qual deletar
    async deleteBurger(id){
      // Requisição exclusiva do JSON serve para deletar o burger do Id pedido
      const req = await fetch(`http://localhost:3000/burgers/${id}`,{
        method:   "DELETE"
      });


      const res = await req.json();
      console.log(res);
      
      //colocar mensagem de sistema
      this.msg = `Pedido Removido!`

      //Limpar mensagem após 3 segundos
      setTimeout(() => this.msg = "",3000);


      this.getPedidos();
    },
    // Requisições para atualizar o status dos burgers
    async updatedBurger(event, id){
      // capturando a opção do select selecionada e convertendo em string e json
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option});

      // requisição para atualizar apenas o dado de status lançado
      const req = await fetch(`http://localhost:3000/burgers/${id}`,{
        method: "PATCH", //ATUALIZA APENAS O QUE ENVIOU
        headers: {"Content-Type": "application/json"},
        body: dataJson
      });

      const res = await req.json();

      //colocar mensagem de sistema
      this.msg = `Pedido Nº${res.id} foi atualizado para ${res.status}!`

      //Limpar mensagem
      setTimeout(() => this.msg = "",3000);
    }
  },
  mounted() {
    // Faz requisição dos dados dos burgers assim que o componente é montado
    this.getPedidos();
  }

}
</script>

<style scoped>
  #burger_table{
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger_table_heading,
  #burger_table_rows,
  .burger_table_row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger_table_heading{
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;

  }

  #burger_table_heading div,
  .burger_table_row div{
    width: 19%;
  }

  .burger_table_row {
    width:100%;
    padding: 12px;
    border-bottom: 1px solid #ccc;    
  }

  #burger_table_heading .order_id,
  .burger_table_row .order_number {
    width:5%;
  }

  select{
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete_btn{
    background-color: #222;
    color: #FCBA03;
    font-weight:bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }

  .delete_btn:hover{
    background-color: transparent;
    color: #222;
  }
</style>