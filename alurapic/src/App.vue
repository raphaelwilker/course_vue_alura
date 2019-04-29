<template>
  <div class="corpo">
    <h1 class="titulo centralizado" v-text="titulo"></h1>
    <!-- 
      : antes do attributo é igual a v-bind.
      O vue trabalha com o mesmo sitema que o angular tem no ng-bind
    -->
    <!-- <img :src="photo.url" :alt="photo.alt"/> -->
    <ul class="lista-photos">

      <li class="lista-photos-item" v-for="foto in fotos">
        <meu-painel :titulo="foto.titulo">
            <img class="imagem-responsiva" :src="foto.url" :alt="foto.titulo">
        </meu-painel>
      </li>

    </ul>
  </div>
</template>

<script>
import Painel from './components/shared/painel/Painel.vue';
export default {
    
  components: {
    'meu-painel': Painel
  },
  
  data(){
    return{
      titulo: 'Alura pic',
      fotos: []
    }
  },

  created(){
    let promise = this.$http.get('http://localhost:3000/v1/fotos');
    promise
      .then(res => res.json())
      .then(fotos => this.fotos = fotos);
  }
}
</script>

<style>
  /* @import url(app.css); */
  
</style>
