<template>

  <div>
    <table class="tabuleiro">

      <tr v-for="(linha, indexLinha) of tabuleiro" :key="'linhaTabuleiro' + indexLinha">
        <th v-for="(coluna, indexColuna) in tabuleiro[indexLinha]" :key="'colunaTabuleiro' + indexColuna" @click="moverPedra(indexLinha, indexColuna)">
          <pedra class="pedra" :numeroPeca="coluna" :class="{'casa_destaque': destaque[indexLinha][indexColuna]}"/>
        </th>
      </tr>

    </table>
  </div>
</template>

<script>

import Pedra from "./components/Pedra.vue"

export default {
  name: 'app',

  data() {
    return {

      /*
        0 - Casa vazia
        1 ou 7 - Peão
        2 - Torre
        3 - Cavalo
        4 - Bispo
        5 - Rainha
        6 - Rei
        *10 - Adversário
      */

     brancaJogando: true,
     orientacaoTabuleiro: 0, // 0 - Brancas para baixo | 1 - Brancas para cima

      tabuleiro: [
        [ 2,  3,  4,  5,  6,  4,  3,  2],
        [ 1,  1,  1,  1,  1,  1,  1,  1],
        [ 0,  0,  0,  0,  0,  0,  0,  0],
        [ 0,  0,  0,  0,  0,  0,  0,  0],
        [ 0,  0,  0,  0,  0,  0,  0,  0],
        [ 0,  0,  0,  0,  0,  0,  0,  0],
        [10, 10, 10, 10, 10, 10, 10, 10],
        [20, 30, 40, 50, 60, 40, 30, 20]
      ],

      destaque: [
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false],
        [false, false, false, false, false, false, false, false]
      ],

    }
  },

  components: {
    pedra: Pedra
  },

  created() {
  },

  methods: {

    moverPedra(linha, coluna) {

      let pedra, brancaSelecionada;

      this.zerarDestaque();

      pedra = this.tabuleiro[linha][coluna];
      brancaSelecionada = pedra % 10 == 0;

      if((brancaSelecionada != this.brancaJogando) || pedra == 0) return;

      pedra = brancaSelecionada ? pedra / 10 : pedra;

      switch(pedra) {

        case 1:
        case 7:

          let loop = (pedra == 1) ? 3 : 2;
          let orientacao = this.orientacaoTabuleiro == 0 ? -1 : 1;
          orientacao = !brancaSelecionada ? orientacao * (-1) : orientacao;

          for(let i = 0; i < loop; i++) {

            this.destaque[linha][coluna] = true;

            linha += orientacao;
          }
          break;
      }

      this.atualizarTabuleiro();

    },

    atualizarTabuleiro() {

      this.tabuleiro.push([]);
      this.tabuleiro.pop();

    },

    zerarDestaque() {

      for(let i = 0; i < 8; i++) {
        for(let j = 0; j < 8; j++) {

          this.destaque[i][j] = false;

        }
      }

    }
  }


}
</script>

<style>

  * {

    margin: 0px;
    padding: 0px;
  }

  .tabuleiro th {
    height: 50px;
    width: 50px;
    border: 1px solid black;
    cursor: pointer;
  }

  .tabuleiro tr:nth-child(odd) th:nth-child(odd), .tabuleiro tr:nth-child(even) th:nth-child(even) {
    background-color: white;
    color: black;
  }

  .tabuleiro tr:nth-child(odd) th:nth-child(even), .tabuleiro tr:nth-child(even) th:nth-child(odd) {
    background-color: gray;
    color: white;
  }

  .pedra {
    width: 100%;
    height: 100%;
  }

  .casa_destaque {
    background-color: darkcyan;
  }

</style>
