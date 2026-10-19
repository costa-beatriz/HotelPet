<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const API_URL = 'http://localhost:3000';
const tutor = ref({});
const route = useRoute();

const novoPet = ref({
  nome: '',
  especie: ''
});

async function carregarTutor() {
  try {
    const respostaTutor = await fetch(`${API_URL}/tutores/${route.params.id}`);
    tutor.value = respostaTutor.ok ? await respostaTutor.json() : { nome: 'Tutor não encontrado' };
  } catch (error) {
    console.error('Erro ao carregar os dados do tutor:', error);
  }
}

async function cadastrarPet() {
  try {
    await fetch(`${API_URL}/pets`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        nome: novoPet.value.nome,
        especie: novoPet.value.especie,
        tutorId: tutor.value.id
      }),
    });

    novoPet.value.nome = '';
    novoPet.value.especie = '';

    await carregarTutor();
  } catch (error) {
    console.error('Erro ao cadastrar pet:', error);
  }
}

onMounted(carregarTutor);
</script>

<template>
  <div class="container my-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <div>
        <h1 class="text-2xl font-bold">Detalhes do Tutor</h1>
        <p class="text-body-secondary mb-0">Visualize os dados do tutor e cadastre pets vinculados.</p>
      </div>
      <RouterLink to="/tutores" class="btn btn-outline-secondary">
        ← Voltar para Tutores
      </RouterLink>
    </div>

    <div class="card mb-4 p-4">
      <h3 class="font-bold mb-3">Informações do Tutor</h3>
      
      <h5 class="text-muted mb-3">Dados do Tutor</h5>
      <div class="row mb-3">
        <div class="col-md-4">
          <strong>ID</strong>
          <p>{{ tutor.id }}</p>
        </div>
        <div class="col-md-4">
          <strong>Nome Completo</strong>
          <p>{{ tutor.nome }}</p>
        </div>
        <div class="col-md-4">
          <strong>CPF / CNPJ</strong>
          <p>{{ tutor.cpf || tutor.cnpj }}</p>
        </div>
      </div>

      <div class="row mb-4">
        <div class="col-md-4">
          <strong>Telefone / Celular / WhatsApp</strong>
          <p>{{ tutor.telefone }}</p>
        </div>
        <div class="col-md-4">
          <strong>E-mail</strong>
          <p>{{ tutor.email }}</p>
        </div>
        <div class="col-md-4">
          <strong>Data de Nascimento</strong>
          <p>{{ tutor.dataNascimento }}</p>
        </div>
      </div>

      <hr />

      <h5 class="text-muted my-3">Endereço</h5>
      <div class="row mb-3">
        <div class="col-md-4">
          <strong>Rua</strong>
          <p>{{ tutor.rua }}</p>
        </div>
        <div class="col-md-4">
          <strong>Número</strong>
          <p>{{ tutor.numero }}</p>
        </div>
        <div class="col-md-4">
          <strong>Bairro</strong>
          <p>{{ tutor.bairro }}</p>
        </div>
      </div>

      <div class="row">
        <div class="col-md-4">
          <strong>Cidade</strong>
          <p>{{ tutor.cidade }}</p>
        </div>
        <div class="col-md-4">
          <strong>Estado</strong>
          <p>{{ tutor.estado }}</p>
        </div>
        <div class="col-md-4">
          <strong>CEP</strong>
          <p>{{ tutor.cep }}</p>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-md-7">
        <div class="card p-3">
          <div class="d-flex align-items-center mb-3">
            <h4 class="mb-0 me-3">Pets do Tutor</h4>
            <span class="badge bg-primary">
              {{ tutor.pets ? tutor.pets.length : 0 }} pets
            </span>
          </div>

          <table class="table table-striped table-hover">
            <thead>
              <tr>
                <th>ID</th>
                <th>Nome</th>
                <th>Espécie</th>
                <th>Ações</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="!tutor.pets || tutor.pets.length === 0">
                <td colspan="4" class="text-center text-muted">Nenhum pet vinculado</td>
              </tr>
              <tr v-for="pet in tutor.pets" :key="pet.id">
                <td>{{ pet.id }}</td>
                <td>{{ pet.nome }}</td>
                <td>{{ pet.especie }}</td>
                <td>
                  <RouterLink :to="`/pets/${pet.id}`" class="btn btn-sm btn-outline-primary">
                    Visualizar
                  </RouterLink>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="col-md-5">
        <div class="card p-4">
          <h4 class="mb-1">Cadastrar novo pet</h4>
          <p class="text-muted small mb-3">O pet será vinculado automaticamente a {{ tutor.nome }}.</p>

          <form @submit.prevent="cadastrarPet">
            <div class="mb-3">
              <label for="nomePet" class="form-label">Nome do pet</label>
              <input
                type="text"
                id="nomePet"
                v-model="novoPet.nome"
                class="form-control"
                placeholder="Digite o nome do pet"
                required
              />
            </div>

            <div class="mb-3">
              <label for="especiePet" class="form-label">Espécie</label>
              <select
                id="especiePet"
                v-model="novoPet.especie"
                class="form-control"
                required
              >
                <option value="" disabled selected>Selecione</option>
                <option value="Cachorro">Cachorro</option>
                <option value="Gato">Gato</option>
                <option value="Outro">Outro</option>
              </select>
            </div>

            <button type="submit" class="btn btn-success w-100">
              Cadastrar pet
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>