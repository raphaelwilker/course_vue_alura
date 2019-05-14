<template>
  <div >
    <h1 class="titulo centralizado" v-text="titulo"></h1>
    
    <!-- novo elemento para exibir mensagens para o usuário -->
    <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>
    
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
            <imagem-responsiva v-meu-transform:scale.animate.reverse="1.2" :url="foto.url" :titulo="foto.titulo"/>
            <router-link :to="{name: 'altera', params: {id: foto._id}}">
              <meu-botao 
                rotulo="Alterar" 
                tipo="button"
                estilo="padrao"  
                />
            </router-link>
            <meu-botao 
              rotulo="Remover" 
              tipo="button"  
              @botaoAtivado="remove(foto)"
              :confirmacao="true"
              estilo="perigo"
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
import FotoService from '../../domain/foto/FotoService';

export default {
    
  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao
  },

  methods:{

    remove(foto) {
      this.service
      .apaga(foto._id)
      .then(() =>{
        let indice = this.fotos.indexOf(foto); // acha a posição da foto na lista
        this.fotos.splice(indice, 1); // a instrução altera o array
        this.mensagem = 'Foto removida com sucesso';
      },
      err => {
          this.mensagem = 'Não foi possível remover a foto';
          console.log(err);
      })
          

    }


  },

  data(){
    return{
      titulo: 'Alura pic',
      fotos: [],
      filtro: '',
      mensagem: ''
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
    
    this.service = new FotoService(this.$resource);

    this.service
      .lista()
      .then(fotos => this.fotos = fotos, err => this.mensagem = err.message);

    /*
    let promise = this.$http.get('v1/fotos');
    promise
      .then(res => res.json())
      .then(fotos => this.fotos = fotos);
      */
  }
}
</script>

<style>
  /* @import url(app.css); */
  
</style>
