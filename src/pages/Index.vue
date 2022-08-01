<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h2>{{name}}</h2>
      <q-img :src="url" width="250px"/>
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" style="text-transform: lowercase" label="Digite Nome Pokemon"/>
      <q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
    </div>
    <div class="row justify-between full-width absolute container-arrows">
      <q-icon name="far fa-arrow-alt-circle-left"
        color="primary"
        class="q-ml-sm cursor-pointer"
        @click='getPokemon(id-1)'
        size="50px">
        <q-tooltip>Anterior</q-tooltip>
        </q-icon>
      <q-icon name="far fa-arrow-alt-circle-right"
        color="primary"
        class="q-mr-sm cursor-pointer"
        @click='getPokemon(id+1)'
        size="50px">
        <q-tooltip>Proximo</q-tooltip>
        </q-icon>
    </div>
  </q-page>
</template>

<script>
import api from '../services/api';
export default {
  name: 'PageIndex',

  data(){
    return {
      name:'',
      url:'',
      search: '1',
      id:null
    }
  },

  async beforeMount(){
    await this.getPokemon();
  },

  methods:{
    getPokemon(id) {
      api
      .get(id > 0 ? `/pokemon/${id}/`: `/pokemon/${this.search}/`)
      .then((response) => {
      // handle success
      this.id = response.data.id;
      this.name = response.data.name;
      this.search = response.data.name
      this.url = response.data.sprites.other.dream_world.front_default;
      })
      .catch((error) => {
        this.triggerNegative();
      })
      .then(() => {
        // always executed
      });
    },

    triggerPositive(){
      this.$q.notify({
        type: 'positive',
        position: 'top',
        message: 'Pokemon Encontrado'
      })
    },

    triggerNegative(){
      this.$q.notify({
        type: 'negative',
        position: 'top',
        message: 'Ocorreu um erro, tente novamente'
      })
    },
  }
}
</script>

<style lang="scss" scoped>
  .container-arrows{

  }
</style>
