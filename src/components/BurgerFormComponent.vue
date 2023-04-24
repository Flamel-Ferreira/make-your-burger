<template>
  <div>
    <!-- Pop-up de feedback -->
    <MessageComponent :msg="msg" v-show="msg"/>
    <div>
      <!-- Formulário que chama a função createBurguer quando enviada -->
      <form id="Burger_form" @submit="createBurger">
        <!-- Input Nome: -->
        <div class="input_container">
          <label for="nome">Nome do Cliente:</label>
          <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite seu nome">
        </div>
        <!-- Input Pão -->
        <div class="input_container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao">
            <option value="">Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
          </select>
        </div>
        <!-- Input Carne -->
        <div class="input_container">
          <label for="carne">Escolha a carne:</label>
          <select name="carne" id="carne" v-model="carne">
            <option value="">Selecione a carne</option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
          </select>
        </div>
        <!-- Select Opcionais -->
        <div id="opcionais_container" class="input_container">
          <label id="opcionais_title" for="opcinais">Escolha os opcionais:</label>
          <div class="checkbox_container" v-for="opcional in opcionaisdata" :key="opcional.id">
            <input type="checkbox" name="opcionais" id="opcionais" :value="opcional.tipo" v-model="opcionais">
            <span>{{opcional.tipo}}</span>
          </div>
        </div>
        <!-- Botão Submit -->
        <div class="input_container">
          <input type="submit" value="Crie meu Burger!" class="submit_btn">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import MessageComponent from "./MessageComponent.vue"
export default{
  components:{
    MessageComponent
  },
  data(){
    return{
      paes:null,
      carnes:null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne:null,
      opcionais:[],
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    // função assincrona devido trabalhar com requisições, que tem como resposta uma promise.
    async getIngredientes() {
      // await espera a requisição que esta sendo feita terminar para executar o restante do código
      // requisição para o fake backend json serve
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      // armazenando os valores da resposta da requisição no data
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createBurger(e){
      // impedindo o comportamento padrão do formulário, que é recarregar a página quando enviado.
      e.preventDefault();
      
      // constuindo um objeto para enviar para o servidor
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao:this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      // convertendo o objeto em string e posteriormente em json
      const dataJson = JSON.stringify(data);

      // enviando os o objeto para o servidor
      const req = await fetch("http://localhost:3000/burgers",{
        method:"POST",
        headers:{ "Content-Type": "application/json"},
        body: dataJson
      });

      // transformando a resposta do envio de dados em json
      const res = await req.json();
      console.log(res);

      // Definindo mensagem do pop up
      this.msg = `Pedido Nº ${res.id} realizado com sucesso!`

      //Limpar mensagem em 3 segundos
      setTimeout(() => this.msg = "",3000);

      //limpar campos para novo cadastro
      this.nome = "";
      this.carne = "";
      this.pao = "";
      this.opcionais = "";
    }
  },
  mounted(){
    // chamando getIngredientes ao montar a aplicação
    this.getIngredientes();
  }
}
</script>

<style scoped>
 #Burger_form{
  max-width: 400px;
  margin: 0 auto;
 }

 .input_container{
  display:flex;
  flex-direction: column;
  margin-bottom: 20px;
 }

 label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #FCBA03;
 }

 input, select{
  padding: 5px 10px;
  width: 100%;
 }

 #opcionais_container{
  flex-direction: row;
  flex-wrap: wrap;
 }

 #opcionais_title{
  width: 100%;
 }

 .checkbox_container{
  display:flex;
  align-items: center;
  width:50%;
  margin-bottom: 20px;
 }

 .checkbox_container span,
 .checkbox_container input{
  width: auto;
 }

 .checkbox_container span{
  margin-left: 6px;
  font-weight: bold;
 }
 
 .submit_btn{
  background-color: #222;
  width:70%;
  color: #FABA03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor:pointer;
  transition: .5s;
 }

 .submit_btn:hover{
  background-color: transparent;
  color: #222;
 }
</style>