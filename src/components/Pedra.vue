<template>

  <div>

     <img v-if="numeroPeca != 0" :src="sprite"/>

  </div>

</template>

<script>
export default {
  name: 'Peca',
  props: ['numeroPeca'],

  computed: {
     sprite: function() {

        let chavePeca, tipo;
        let numero = this.numeroPeca;

        chavePeca = {
           c1: "peao",
           c2: "torre",
           c3: "cavalo",
           c4: "bispo",
           c5: "rainha",
           c6: "rei",
           c7: "peao"
        }

        if(numero % 10 == 0) {

           tipo = "branco";
           numero /= 10;

        } else {
           tipo = "preto";
        }

        return require("../assets/pecas/" + chavePeca["c" + numero] + "_" + tipo + ".png");
     }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

   div img {
      height: 100%;
      width: 100%;
   }

</style>