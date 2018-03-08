<template>
  <div id="app">

    <div class="col-xs-6 text-center" v-if="!isGameActive">

      <button class="btn btn-primary" @click="startGame"><h2>Start game</h2></button>

    </div>

    <div class="col-xs-6 text-center" v-if="isGameActive">

      <button class="btn btn-light" @click="giveUp"><h3>Give Up</h3></button>
      <button class="btn btn-light"><h3>Reset</h3></button>

      <div class="game-board">

        <table>
          <tr v-for="row in 6">
            <td class="cell" v-for="column in 7"><div id="circle"></div></td>
          </tr>
        </table>


      </div>
      <div class="table-end-board"></div>

    </div>

  </div>
</template>

<script>
  export default {
      name: 'App',
      data: function () {
          return {
              'isGameActive': false
          }
      },
      methods: {
          startGame: function () {
              this.isGameActive = true;
          },
          giveUp: function () {
              this.isGameActive = false;
          }
      }
  }
</script>

<style>
  #app {
    padding-left: 20%;
    padding-right: 20%;
  }

  .cell {
    width: 100px;
    height: 100px;
    background-color: blue;
    border: 1px solid;
  }

  .game-board {
    padding-right: 20%;
    padding-left: 20%;
    padding-top: 5%;
  }
  #circle:hover {
    background-color: chartreuse;
  }

  .table-end-board {
    height: 10px;
    width: 100%;
    background-color: sienna;
    box-shadow: 0px 5px 5px grey;
  }
  #circle {
    width: 100px;
    height: 100px;
    background: aliceblue;
    -moz-border-radius: 50px;
    -webkit-border-radius: 50px;
    border-radius: 50px;
  }
</style>
