<template>
  <div>
    <v-img src="/img/carpark.jpg" style="height: 50vw; min-height: 400px; max-height:600px;">
    <v-row
      style="background-color: rgba(50, 50, 50, 0.8); height: 100%;"
      align="center"
      justify="center"
    >
        <div class="py-4 px-8 d-flex text-center flex-column align-center" style="width: 80%;">
          <h1 id="biljhälpen-title" class="display-1 font-weight-thin mb-4 white--text">Bilhjälpen hjälper dig hitta din nya bil</h1>
          <v-btn color="primary" id="visa-alla-bilar" class=" white--text mt-8" to="/bilar" tile>Visa alla bilar</v-btn>
        </div>
    </v-row>
    </v-img>

    <v-container>
      <h2 id="popular-categories" class="text-center headline my-6">De populäraste kategorierna</h2>    
      <v-row class="fill-height" align="center" justify="center">
        <CategoryButton
          v-for="(item, index) in items"
          v-bind:item="item"
          v-bind:key="index" />
      </v-row>
    </v-container>
  </div>
</template>

<script>
import CategoryButton from '../components/CategoryButton.vue'

export default {
  name: "Home",
  metaInfo: {
  },

  components: {
    CategoryButton
  },

  data: () => ({
      url: '',
      items: [
        {
          title: 'För den miljövänliga',
          text: "Miljövänliga hybridbilar, elbilar och gasbilar",
          color: 'green',
          link: '/kategori/miljo'
        },
        {
          title: 'För den riktigt miljövänliga',
          text: 'Endast de renaste elbilarna',
          color: 'red',
          link: '/kategori/el'
        }
      ]
  }),
  created() {

  },
  methods: {

  },
};
</script>
<style scoped>

</style>
