<script setup>
  const props = defineProps(['tarefas'])
</script>

<template>
  <ul class="list-group mt-4" v-if="props.tarefas.length > 0">
    <li  
      class="list-group-item" 
      v-for="tarefa in props.tarefas"
    >
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
  <p class="mt-4" v-else>Não há tarefas.</p>
</template>

<style scoped>
.done {
    text-decoration: line-through;
  }
</style>