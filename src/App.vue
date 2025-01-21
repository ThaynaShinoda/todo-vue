<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar E6S6',
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
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você tem {{ getTarefasPendentes().length}} tarefas pendentes</p>
    </header>
    <!-- e.preventDefault() = @submit.prevent=""-->
    <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input 
          :value="estado.tarefaTemp"
          @change="event => estado.tarefaTemp = event.target.value"
          required
          type="text" 
          placeholder="Digite a tarefa" 
          class="form-control"
        >
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">
          Cadastrar
        </button>
      </div>
      <div class="col-md-2">
        <select @change="event => estado.filtro = event .target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
  
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input 
        @change="event => tarefa.finalizada = event.target.checked"
        :checked="tarefa.finalizada" 
        :id="tarefa.titulo"
        type="checkbox"
      >
      <label 
        class="ms-3" 
        :class="{ done: tarefa.finalizada }"
        :for="tarefa.titulo"
      >
        {{tarefa.titulo}}
      </label>
    </li>
  </ul>
  </div>
  
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
