<template>
  <div>
    <h1 v-text="titulo"></h1>
    <!-- 
      : antes do attributo é igual a v-bind.
      O vue trabalha com o mesmo sitema que o angular tem no ng-bind
    -->
    <!-- <img :src="photo.url" :alt="photo.alt"/> -->
    <ul>

        <li v-for="foto in fotos">
            <img :src="foto.url" :alt="foto.titulo">
        </li>

        

    </ul>
  </div>
</template>

<script>
export default {
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
</style>
