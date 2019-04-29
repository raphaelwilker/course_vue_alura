<template>

    <div class="painel">
        <h2 class="painel-titulo" @dblclick="visivel = !visivel">{{titulo}}</h2>
        <figure v-show="visivel">
	        <slot class="painel-corpo" >
	        </slot><!-- fim painel-corpo -->
        </figure>
        
    </div><!-- fim painel -->


</template>

<script>
export default {

  props: ['titulo'],

  data() {

       return {

           visivel: true
       }

   }

}
</script>
