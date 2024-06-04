<template>
    <div class="about">
      <h1>Encomendas</h1>

    <!-- Campos de entrada para o cadastro de encomendas -->
    <p><label for="rastreio">Rastreio: </label><input id="rastreio" type="int" v-model="encomenda.rastreio"/></p>
    <p><label for="conteudo">Conteúdo: </label><input id="conteudo" type="text" v-model="encomenda.conteudo"/></p>
    <p><label for="data_hora_prevista">Data e Hora Prevista: </label><input id="dataHoraPrevista" type="datetime-local" v-model="encomenda.dataHoraPrevista"/></p>
    <p><label for="data_hora_entrega">Data e Hora Entrega: </label><input id="dataHoraEntrega" type="datetime-local" v-model="encomenda.dataHoraEntrega"/></p>
    <button @click="incluir">Incluir</button>

    <!-- Exibição da lista de encomendas -->
    <div v-if="encomendas.length === 0 && erro">
      <p>{{ erro }}</p>
    </div>
    <table v-else>
      <thead>
        <tr>
            <td>Rastreio</td>
            <td>Conteudo</td>
            <td>Data e Hora Prevista</td>
            <td>Data e Hora Entrega</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="encomenda in encomendas" :key="encomenda.rastreio">
          <td>{{ encomenda.rastreio }}</td>
          <td>{{ encomenda.conteudo }}</td>
          <td>{{ encomenda.dataHoraPrevista }}</td>
          <td>{{ encomenda.dataHoraEntrega }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="limparConsulta">Mostrar Todos</button>
</div>
  </template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import axios from 'axios';

// Definição dos dados da encomenda
const encomenda = ref({
    rastreio: null,
    conteudo: '',
    dataHoraPrevista: '',
    dataHoraEntrega: '',
});

// // Definição dos dados para consulta
// const consultaConteudo = ref('');
// const consultaData = ref<dateTime-local | null>(null);

// Definição da lista de encomendas e mensagem de erro
const encomendas = ref([]);
const erro = ref('');

// Função para incluir um nova encomenda
async function incluir() {
  erro.value = '';
  try {
    await axios.post('encomenda', encomenda.value);
    limparCampos(); // Limpar os campos após o cadastro
    buscarTodasEncomendas(); // Atualizar a lista
  } catch (e) {
    erro.value = (e as Error).message;
  }
}

// Função para limpar os campos do formulário
function limparCampos() {
  encomenda.value = {
    id: '',
    rastreio: null,
    conteudo: '',
    dataHoraPrevista: '',
    dataHoraEntrega: '',
  };
}

// Função para buscar todas as encomendas
async function buscarTodasEncomendas() {
  erro.value = '';
  try {
    const response = await axios.get('encomenda');
    encomendas.value = response.data;
  } catch (e) {
    erro.value = (e as Error).message;
  }
}

// // Função para limpar a consulta e mostrar todas as encomendas
// function limparConsulta() {
//   consultaTermo.value = '';
//   consultaVersao.value = null;
//   erro.value = '';
//   buscarTodasEncomendas();
// }

// Montagem inicial do componente
onMounted(() => {
  buscarTodasEncomendas();
});
</script>