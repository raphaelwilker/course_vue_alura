<template>
	<div>
		<h1 v-if="foto._id" class="centralizado">Alteração</h1>
    	<h1 v-else class="centralizado">Inclusão</h1>
		<h2 class="centralizado">{{ foto.titulo }}</h2>

		<form @submit.prevent="grava()">
		  <div class="controle">
		    <label for="titulo">TÍTULO</label>
		    <input  
		    	id="titulo"
					name="titulo" 
		    	autocomplete="off" 
		    	v-model="foto.titulo"
					v-validate
					data-vv-rules="required|min:3|max:30"
		    	>
					<span v-show="errors.has('titulo')" class="erro">{{ errors.first('titulo') }}</span>
		  </div>

		  <div class="controle">
		    <label for="url">URL</label>
		    <input 
		    	id="url"
					name="url"
		    	autocomplete="off"
		    	v-model="foto.url"
					v-validate
					data-vv-rules="required|min:3|max:30"
		    >
				<span class="erro" v-show="errors.has('url')">
						{{ errors.first('url') }}
				</span>
		    <imagem-responsiva 
		    	v-show="foto.url"
		    	:url="foto.url"
		    	:titulo="foto.titulo"
		    />
		  </div>

		  <div class="controle">
		    <label for="descricao">DESCRIÇÃO</label>
		    <textarea
		    	id="descricao"
		    	autocomplete="off"
		    	v-model="foto.descricao"
		    ></textarea>
		  </div>

		  <div class="centralizado">
		    <meu-botao rotulo="GRAVAR" tipo="submit" estilo="padrao"/>
		    <router-link :to="{name: 'home'}"><meu-botao rotulo="VOLTAR" tipo="button" estilo="padrao"/></router-link>
		  </div>

		</form>
	</div>
</template>

<script>

import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue';

import Foto from '../../domain/foto/Foto.js';
import FotoService from '../../domain/foto/FotoService';

export default {

	components: {

		'imagem-responsiva': ImagemResponsiva, 
		'meu-botao': Botao
	},

	data() {
	    return {

	      foto: new Foto(),
	      id: this.$route.params.id

	    }
	},

	methods: {

		grava() {
			this.$validator
			.validateAll()
			.then(success => {
				if(success){
					this.service
					.cadastra(this.foto)
					.then(() => {
						if(this.id) this.$router.push({ name: 'home'});
						this.foto = new Foto()
					},
						err => console.log(err)
					);
				}
			})
			
			/*
			this.$http
	        .post('v1/fotos', this.foto)
	        .then(() => this.foto = new Foto(), err => console.log(err));
	        */
		}

	},

	created() {
		this.service = new FotoService(this.$resource);

		if(this.id){
			this.service
				.busca(this.id)
				.then(foto => this.foto = foto);
		}
	}
}

</script>
<style scoped>
  

</style>