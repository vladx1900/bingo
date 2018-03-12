<template>
  <div id="app">

    <div class="col-xs-6 text-center" v-if="!isGameActive">

      <button class="btn btn-primary" @click="startGame"><h2>Start game</h2></button>

    </div>

    <div class="col-xs-6 text-center" v-if="isGameActive">

      <button class="btn btn-light" @click="giveUp"><h3>Give Up</h3></button>
      <button class="btn btn-light"><h3>Reset</h3></button>

      <div class="game-board">

        <table id="table">
          <tr v-for="row in 7">
            <template v-if="row === 1">
              <td class="arrow invisible" v-for="column in 7"><img src="./assets/arrow.png" style="height: 40px; width: 70px;"></td>
            </template>
            <template v-if="row !== 1">
              <td class="cell" v-for="column in 7">
                <div class="circle" :style="{'background-color': circleColorInit}" @mouseover="showArrow(column)" @mouseleave="hideArrow(column)" @click="markCell($event, row, column)"></div>
              </td>
            </template>

          </tr>
        </table>


      </div>
      <div class="table-end-board"></div>

      <div>
        <template v-if="player1"><h4>RED player's turn:</h4></template>
        <template v-if="player2"><h4>YELLOW player's turn:</h4></template>
      </div>

    </div>

  </div>
</template>

<script>
  export default {
      name: 'App',
      data: function () {
          return {
              'isGameActive': false,
              'matrix': [
                      [0,0,0,0,0,0,0],
                      [0,0,0,0,0,0,0],
                      [0,0,0,0,0,0,0],
                      [0,0,0,0,0,0,0],
                      [0,0,0,0,0,0,0],
                      [0,0,0,0,0,0,0]
                  ],
              player1: true,
              player2: false,
              circleColorInit: 'aliceblue'
          }
      },
      methods: {
          startGame: function () {
              this.isGameActive = true;
          },
          giveUp: function () {
              this.isGameActive = false;
          },
          showArrow: function (column) {
              var cell = this.selectCell(1, column);
              cell.setAttribute('class', 'arrow');
          },
          hideArrow: function (column) {
              var cell = this.selectCell(1, column);
              cell.setAttribute('class', 'arrow invisible');
          },
          markCell: function (event, row, column) {

              if ( ((row-2) === 5 || this.matrix[row-1][column-1] !== 0) && this.isGameActive) {

                  if (this.matrix[row - 2][column - 1] === 0) {

                      if (this.player1) {
                          this.matrix[row - 2][column - 1] = 1;
                          event.target.style.backgroundColor = 'red';
                          this.checkForWinner(row-2, column-1, 1);
                          this.player1 = false;
                          this.player2 = true;
                      } else {
                          this.matrix[row - 2][column - 1] = 2;
                          event.target.style.backgroundColor = 'yellow';
                          this.checkForWinner(row-2, column-1, 2);
                          this.player1 = true;
                          this.player2 = false;
                      }
                  }

              } else {
                  console.log("error");
              }

          },
          selectCell: function (row, column) {
              var table = $("#table")[0];
              var cell = table.rows[row-1].cells[column-1];
              return cell;
          },
          checkForWinner: function (row, column, player) {
              var x;
              var y;
              var winCondition;

              //case 1
              x = row;
              y = column;
              winCondition = 1;

              while((y-1)>=0 && this.matrix[x][y-1] === player) {
                  y--;
                  winCondition++;
              }
              y = column;
              while((y+1)<7 && this.matrix[x][y+1] === player) {
                  y++;
                  winCondition++;
              }

              if(winCondition >=4) {
                  this.showSwal(player);
                  return true;
              }

              //case 2
              x= row;
              y = column;
              winCondition = 1;

              while((x-1)>=0 && this.matrix[x-1][y] === player) {
                  x--;
                  winCondition++;
              }
              x = row;
              while((x+1)<6 && this.matrix[x+1][y] === player) {
                  x++;
                  winCondition++;
              }
              if(winCondition >=4) {
                  this.showSwal(player);
                  return true;
              }

              //case 3
              x = row;
              y = column;
              winCondition = 1;

              while((x-1)>=0 && (y+1)<7 && this.matrix[x-1][y+1] === player) {
                  x--;
                  y++;
                  winCondition++;
              }
              x = row;
              y = column;
              while((x+1)<6 && (y-1)>=0 && this.matrix[x+1][y-1] === player) {
                  x++;
                  y--;
                  winCondition++;
              }
              if(winCondition >= 4) {
                  this.showSwal(player);
                  return true;
              }

              //case 4
              x = row;
              y = column;
              winCondition = 1;

              while((x-1)>=0 && (y-1)>=0 && this.matrix[x-1][y-1] === player) {
                  x--;
                  y--;
                  winCondition++;
              }
              x = row;
              y = column;
              while((x+1)<6 && (y+1)<7 && this.matrix[x+1][y+1] === player) {
                  x++;
                  y++;
                  winCondition++;
              }
              if(winCondition >= 4) {
                  this.showSwal(player);
                  return true;
              }

              return false;




          },
          showSwal: function (player) {
              if(player === 1) {
                  swal('RED player wins!');
              } else {
                  swal('YELLOW player wins!');
              }
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
    border: 2px solid;
  }

  .arrow {
    width: 130px;
    height: 70px;
  }

  .game-board {
    padding-right: 20%;
    padding-left: 20%;
    padding-top: 5%;
  }
  .circle:hover {
    background-color: chartreuse !important;
  }

  .table-end-board {
    height: 10px;
    width: 100%;
    background-color: sienna;
    box-shadow: 0px 5px 5px grey;
  }
  .circle {
    width: 100px;
    height: 100px;
    border-radius: 50%;
  }

  .circle-red {
    width: 100px;
    height: 100px;
    background-color: sienna;
    -moz-border-radius: 50px;
    -webkit-border-radius: 50px;
    border-radius: 50px;
  }
</style>
