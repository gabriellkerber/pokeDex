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
    }
  },

  async beforeMount(){
    await this.getPokemon();
  },

  methods:{
    getPokemon() {
      api
      .get(`/pokemon/${this.search}/`)
      .then((response) => {
      // handle success
      console.log(response);
      this.name = response.data.name;
      this.url = response.data.sprites.other.dream_world.front_default;
      this.triggerPositive();
      })
      .catch((error) => {
        // handle error
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
