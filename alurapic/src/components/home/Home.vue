<template>
  <div >
    <h1 class="titulo centralizado" v-text="titulo"></h1>

    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="filtre pelo título da foto">
    {{filtro}}
    <!-- 
      : antes do attributo é igual a v-bind.
      O vue trabalha com o mesmo sitema que o angular tem no ng-bind
    -->
    <!-- <img :src="photo.url" :alt="photo.alt"/> -->
    <ul class="lista-photos">

      <li class="lista-photos-item" v-for="foto of fotosComFiltro">
        <meu-painel :titulo="foto.titulo">
            <imagem-responsiva :url="foto.url" :titulo="foto.titulo"/>
            <meu-botao 
              rotulo="Remover" 
              tipo="button"  
              @botaoAtivado="remove(foto)"
              :confirmacao="false"
              estilo="padrao"
              />
        </meu-painel>
      </li>

    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';

export default {
    
  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
  },

  methods:{

    remove(foto) {

      alert(foto.titulo);
    }


  },

  data(){
    return{
      titulo: 'Alura pic',
      fotos: [],
      filtro: ''
    }
  },

  computed: {
    fotosComFiltro() {
      if (this.filtro) {
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        return this.fotos;
      }
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
