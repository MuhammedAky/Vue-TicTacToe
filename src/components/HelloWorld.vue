<template>
  <div class="game-board">
    <table cellspacing="0" v-if="!isdone">
      <tr v-for="(row, rowIndex) in game">
        <td
          @click="oyna(rowIndex, columnIndex)"
          v-for="(column, columnIndex) in row"
        >
          {{ game[rowIndex][columnIndex] }}
        </td>
      </tr>
    </table>
    <div v-else-if="winner">
      <p>Dostluk Kazandı</p>
    </div>
    <div v-else>
      <h1>Oyun bitti</h1>
      <p>
        Kazanan: 
        {{ turn == "O" ? "X" : "O" }}
      </p>
    </div>
    <button @click="playAgain" class="btn btn-danger mt-3">Tekrar Oyna</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      game: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      turn: "X",
      isdone: false,
      countMove: 0,
      winner: "",
    };
  },
  methods: {
    playAgain: function playAgain() {
      this.game = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.countMove = 0;
      this.turn = "X";
      this.winner = "";
      this.isdone = false;
    },
    oyna: function oyna(rowIndex, columnIndex) {
      // console.log(`row: ${rowIndex} column: ${columnIndex}`);
      if (this.turn == "X" && this.game[rowIndex][columnIndex] == "") {
        this.game[rowIndex][columnIndex] = "X";
        this.$forceUpdate();
        this.isDone();
        this.changeTurn();
      } else if (this.turn == "O" && this.game[rowIndex][columnIndex] == "") {
        this.game[rowIndex][columnIndex] = "O";
        this.$forceUpdate();
        this.isDone();
        this.changeTurn();
      }
      // console.log(this.game[rowIndex][columnIndex]);
    },
    changeTurn: function changeTurn() {
      this.countMove += 1;
      if (this.turn == "X") {
        this.turn = "O";
      } else {
        this.turn = "X";
      }
    },
    isDone: function isDone() {
      for (let i = 0; i < 3; i++) {
        // Sağdan sola tüm satırlar kontrolü
        if (
          this.game[i][0] != "" &&
          this.game[i][1] != "" &&
          this.game[i][2] != "" &&
          this.game[i][0] == this.game[i][1] &&
          this.game[i][1] == this.game[i][2]
        ) {
          this.isdone = true;
          return true;
        } else if (
          // Yukarıdan aşağı tüm sütünlar kontrolü

          this.game[0][i] != "" &&
          this.game[1][i] != "" &&
          this.game[2][i] != "" &&
          this.game[0][i] == this.game[1][i] &&
          this.game[0][i] == this.game[2][i]
        ) {
          this.isdone = true;
          return true;
        }
      }

      // Sol Çaprazdan sağ alta kontrol
      if (
        this.game[0][0] != "" &&
        this.game[1][1] != "" &&
        this.game[2][2] != "" &&
        this.game[0][0] == this.game[1][1] &&
        this.game[0][0] == this.game[2][2]
      ) {
        this.isdone = true;
        return true;
      } else if (
        // Sağ Çaprazdan sol alta kontrol

        this.game[0][2] != "" &&
        this.game[1][1] != "" &&
        this.game[2][0] != "" &&
        this.game[0][2] == this.game[1][1] &&
        this.game[0][2] == this.game[2][0]
      ) {
        this.isdone = true;
        return true;
      } else if (this.countMove == 8) {
        this.winner = "Dostluk";
        this.isdone = true;
        return true;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.game-board table {
  margin: 0 auto;
}
.game-board td {
  font-size: 30px;
  width: 60px;
  height: 60px;
  line-height: 60px;
  text-align: center;
}
.game-board tr:first-child td,
.game-board tr:nth-child(2) td {
  border-bottom: 1px solid #ccc;
}
.game-board tr td:first-child,
.game-board tr td:nth-child(2) {
  border-right: 1px solid #ccc;
}
.reset-button {
  border: none;
  background-color: pink;
  padding: 10px;
  margin-top: 20px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
