<script setup>
  import { onMounted, ref } from 'vue';
  import {useRoute } from 'vue-router';

 // chamando a minha API para exibir os dados de tutor
  const API_URL = 'http://localhost:3000';
  const router = useRoute();

  // chamando a minha API para exibir os dados de tutor
  const tutores = ref([]);

  async function carregarTutores() {
    const resposta = await fetch(`${API_URL}/tutores`);
    console.log('tutores', resposta);
    // transformando os valores da minha API para o fromato JSON
    tutores.value = await resposta.json();
  }

  // chamando a minha API para salvar os dados de tutor
  const novoTutor = ref({
    tutorId: '',
    nome: '',
    cpf: '',
    cnpj: '',
    telefone: '',
    email:'',
    dataNascimento: '',
    rua: '',
    numero: '',
    bairro: '',
    cidade: '',
    estado: '',
    cep: '',
    estado:''
   
  });

  async function salvarTutor() {
    // fazendo a requisição para a API para o servidor
    await fetch(`${API_URL}/tutores`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(novoTutor.value),
    });
    router.push({'name': 'tutores'});
   

    // limpando os campos do formulário
    novoTutor.value = {
    tutorId: '',
    nome: '',
    cpf: '',
    cnpj: '',
    telefone: '',
    email:'',
    dataNascimento: '',
    rua: '',
    numero: '',
    bairro: '',
    cidade: '',
    estado: '',
    cep: '',
    estado:''
    };
  }

  onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secondary mb-0">Preencha os dados para cadastrar novo tutor.</p>
    </header>

    <!--
    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutor
    </RouterLink>
    -->


    <form @submit.prevent="salvarTutor">
      <div class="col-md-6 mb-3">
        <label for="tutorId" class="form-label">Id:</label>
        <h1 class="form-control" required>Gerado Automaticamente</h1>
      </div>

      <div class="col-md-6 mb-3">
        <label for="nome" class="form-label">Nome Completo:</label>

        <input
          type="text"
          id="nome"
          v-model="novoTutor.nome"
          class="form-control"
          required 
          
        />
        
      </div>

      <div class="col-md-6 mb-3">
        <label for="cpf" class="form-label">CPF:</label>
        <input
          type="text"
          id="cpf"
          v-model="novoTutor.cpf"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6 mb-3">
        <label for="cnpj" class="form-label">CNPJ:</label>
        <input
          type="text"
          id="cnpj"
          v-model="novoTutor.cnpj"
          class="form-control"
        />
      </div>

      <div class="col-md-6">
        <label for="telefone" class="form-label">Telefone:</label>
        <input
          type="text"
          id="telefone"
          v-model="novoTutor.telefone"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="email" class="form-label">Email:</label>
        <input
          type="email"
          id="email"
          v-model="novoTutor.email"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="dataNascimento" class="form-label">Data de Nascimento:</label>
        <input
          type="text"
          id="dataNascimento"
          v-model="novoTutor.dataNascimento"
          class="form-control"
          required
        />
      </div>

<div class="col-md-6">
        <h1 class="text-2xl font-bold">Endereço</h1>
</div>

     <div class="col-md-6">
        <label for="rua" class="form-label">Rua:</label>
        <input
          type="text"
          id="rua"
          v-model="novoTutor.rua"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="numero" class="form-label">Número:</label>
        <input
          type="number"
          id="numero"
          v-model="novoTutor.numero"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="bairro" class="form-label">Bairro:</label>
        <input
          type="text"
          id="bairro"
          v-model="novoTutor.bairro"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="cidade" class="form-label">Cidade:</label>
        <input
          type="text"
          id="cidade"
          v-model="novoTutor.cidade"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="cidade" class="form-label">Cidade:</label>
        <input
          type="text"
          id="cidade"
          v-model="novoTutor.cidade"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="estado" class="form-label">Estado:</label>
        <input
          type="text"
          id="estado"
          v-model="novoTutor.estado"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label for="cep" class="form-label">CEP:</label>
        <input
          type="text"
          id="cep"
          v-model="novoTutor.cep"
          class="form-control"
          required
        />
      </div>

    
      <button type="submit" class=" btn btn-success">Cadastrar Tutor</button>
    </form>
  </div>
</template>
