<template>

    <div class="painel">
        <h2 class="painel-titulo" @dblclick="visivel = !visivel">{{titulo}}</h2>
        <transition name="painel-fade">
	        <figure v-show="visivel">
		        <slot class="painel-corpo" >
		        </slot><!-- fim painel-corpo -->
	        </figure>
        </transition>
        
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
