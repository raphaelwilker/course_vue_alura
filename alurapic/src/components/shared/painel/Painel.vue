<template>

    <div class="painel">
        <h2 class="painel-titulo">{{titulo}}</h2>
        <slot class="painel-corpo">
        </slot><!-- fim painel-corpo -->
    </div><!-- fim painel -->


</template>

<script>
export default {

  props: ['titulo']

}
</script>
