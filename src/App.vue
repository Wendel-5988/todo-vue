<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const estado = reactive({
    tarefaTemp: '',
    filtro: 'todas',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false
      },
      {
        titulo: 'Ir a academia',
        finalizada: true
      },
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasConcluidas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'concluidas':
        return getTarefasConcluidas();
      default:
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova)
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho  :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="e => estado.tarefaTemp = e.target.value" :trocar-filtro="e => estado.filtro = e.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas  :tarefas="getTarefasFiltradas()"/>
  </div>
</template>
