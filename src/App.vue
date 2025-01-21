<script setup>
import { reactive } from 'vue';
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizado:false,
      },
      {
        titulo: "Estudar SASS",
        finalizada: false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true,
      }
    ]
    
  })

  function getTarefasPendentes() {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada )
  }

  function getTarefasFinalizadas() {
    return estado.tarefas.filter(tarefa => tarefa.finalizada )
  }

  function getTarefasFiltradas() {
    const { filtro } = estado;
    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas
    }
  }

  function cadastraTarefa() {
    const novaTarefa = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(novaTarefa);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Header :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario 
      :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="event => estado.tarefaTemp = event.target.value"
      :cadastra-tarefa="cadastraTarefa"
      :troca-filtro="event => estado.filtro = event.target.value"
    />
    <ListaDeTarefas 
      :tarefas="getTarefasFiltradas()"
    />
  </div>
  
</template>

<style scoped>

</style>
