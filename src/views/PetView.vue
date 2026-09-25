
<script setup>
import {onMounted, ref} from 'vue'; 'onMounted' 
const API_URL = 'http://localhost:3000';  'API_URL' 

const pets = ref([]); 'pets' 
const tutores = ref([]); 'tutores' 

async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`); //vai trazer a url da api do pets
  pets.value = await respostaPets.json(); // ele espera e depois chama em json

  const respostaTutores = await fetch(`${API_URL}/tutores`); //vai trazer a url da api dos tutores
  tutores.value = await respostaTutores.json(); // ele espera e depois chama em json
}
onMounted(() => {
  carregarDados();
});

</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>
</div>
<table>
  <thead>
    <th>ID</th>
    <th>Nome</th>
    <th>Espécie</th>
    <th>Tutor</th>
  </thead>
  <tbody>
    <tr v-for="pet in pets" :key="pet.id">
      <td>{{ pet.id }}</td>
      <td>{{ pet.nome }}</td>
      <td>{{ pet.especie }}</td>
      <td>
        {{ tutores.find(tutor => tutor.id === pet.tutorId)?.nome || 'Tutor não encontrado' }}
      </td>
    </tr>
  </tbody>
</table>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>
  
</template>