<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Cadastro de Produtos</a>
      </div>
    </nav>

    <div class="container">

      <ul>
        <li v-for="(erro, index) of errors" :key="index">
          campo <b>{{erro.field}}</b> - {{erro.defaultMessage}}
        </li>
      </ul>


      <form @submit.prevent="salvar">

          <label>Código</label>
          <input type="text" placeholder="codigo" v-model="produto.codigo" >
          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="produto.descricao" >
          <label>Tipo</label>
          <input type="text" placeholder="Nome" v-model="produto.tipoProduto" >
          <label>Quantidade Estoque</label>
          <input type="number" placeholder="QTD" v-model="produto.quantidadeEstoque" >
          <label>Valor Fornecedor</label>
          <input type="text" placeholder="Valor" v-model="produto.valorFornecedor" >

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>CODIGO</th>
            <th>DESCRIÇÃO</th>
            <th>TIPO</th>
            <th>QTD</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for="produto of produtos" :key="produto.id">

            <td>{{ produto.codigo }}</td>
            <td>{{ produto.descricao }}</td>
            <td>{{ produto.tipoProduto }}</td>
            <td>{{ produto.quantidadeEstoque }}</td>
            <td>{{ produto.valorFornecedor }}</td>
            <td>
              <button @click="editar(produto)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="remover(produto)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody>
      
      </table>

    </div>

  </div>
</template>

<script>

import Produto from '../services/produtos'

export default {
  name: 'app',
  data () {
    return {
      produto:{
        id: '',
        codigo: '',
        descricao: '',
        tipoProduto: '',
        quantidadeEstoque: '',
        valorFornecedor: ''
      },
      produtos: [],
      errors: [],
      tipoProduto: []
    }
  },

  mounted(){
    this.listar()
  },

  methods:{
    
    listar(){
      Produto.listar().then(resposta => {
        this.produtos = resposta.data
      }).catch(e => {
        console.log(e)
      })
    },

    salvar(){

      if(!this.produto.id){

        Produto.salvar(this.produto).then(resposta => {
          this.produto = {}
          alert('Cadastrado com sucesso!')
          this.listar()
          this.errors = {}
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }else{

        Produto.atualizar(this.produto).then(resposta => {
          this.produto = {}
          this.errors = {}
          alert('Atualizado com sucesso!')
          this.listar()
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }
      

    },

    editar(produto){
      this.produto = produto
    },

    remover(produto){

      if(confirm('Deseja excluir o produto?')){

        Produto.apagar(produto).then(resposta => {
          this.listar()
          this.errors = {}
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }

    }

  }


}

</script>

<style>

</style>