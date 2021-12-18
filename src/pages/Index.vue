<template>
  <q-page class="flex flex-center">
    <!-- <div class="row row justify-around full-width">
        <h2>{{ name }}</h2>
    </div> -->
    <div class="row row justify-around full-width">
        <q-img :src="url" width="250px"/>
    </div>
    <div class="row justify-around full-width ">
      <q-input v-model="search" label="Digite o nome do pokemon" />
      <q-btn
        color="primary"
        label="Pesquisar"
        :size="xs"
        @click="getPokemon()"
      />
    </div>
    <div class="row justify-between absolute full-width container-arrows">
      <q-icon 
        name="far fa-arrow-alt-circle-left" 
        color="primary" 
        size="50px"
        class="q-ml-sm cursor-pointer"
        @click="getPokemon(id - 1)">
        <q-tooltip>Anterior</q-tooltip>
      </q-icon>
      <q-icon 
        name="far fa-arrow-alt-circle-right" 
        color="primary"
        size="50px" 
        class="q-mr-sm cursor-pointer"
        @click="getPokemon(id + 1)">
        <q-tooltip>Próximo</q-tooltip>
      </q-icon>
    </div>
  </q-page>
</template>

<script>
import { useQuasar } from "quasar";
import api from "../services/api";

export default {
  name: "PageIndex",
  data() {
    const $q = useQuasar();

    return {
      name: "",
      url: "",
      id: null,
      search: "pikachu",

      triggerPositive() {
        $q.notify({
          type: "positive",
          position: "top",
          message: "Pokemon encontrado!",
        });
      },

      triggerNegative() {
        $q.notify({
          type: "negative",
          position: "top",
          message: "Pokemon não encontrado!",
        });
      },

    };
  },

  async beforeMount() {
    await this.getPokemon();
  },
  methods: {
    getPokemon(id) {
      api
        .get(`/pokemon/${ id > 0 ? id : this.search }`)
        .then((resp) => {
          this.name = resp.data.name;
          this.id = resp.data.id;
          this.url = resp.data.sprites.other.dream_world.front_default;
          this.search = this.name;
          // this.triggerPositive();
        })
        .catch((error) => {
          // this.triggerNegative();
        });
    },
  },
};
</script>
<style lang="scss" scoped>
.container-arrows {

}
</style>
