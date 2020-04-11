<template>
  <div class="hello">
    <table>
      <tbody>
        <tr v-for="(row,i) in board" :key="i">
          <td
            v-for="(cell, j) in row"
            :key="`${i}-${j}`"
            :id="`${i}-${j}`"
            v-on:click="rightClick"
          >{{ cell.screen }} {{cell.hasbomb }}</td>
        </tr>
      </tbody>
    </table>
    <p>{{shitLinked}}</p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function() {
    return {
      shitLinked: "Please appear",
      board: this.createNestedArray(9, 6),
      firstClick: true
    };
  },
  methods: {
    randomTrueFalse: function() {
      return Math.random() <= 0.2;
    },
    createNestedArray: function(x, y) {
      let nestedArray = [];
      for (let i = 0; i < y; i++) {
        nestedArray.push([]);
        for (let j = 0; j < x; j++) {
          //true represents bomb
          let hasbomb = this.randomTrueFalse();
          //information of individual cell
          nestedArray[i][j] = {
            screen: "?",
            hasbomb: hasbomb,
            hasBeenClicked: false,
            nearbyBombs: 0,
            id: `${j}-${i}`,
            wasrightClicked: false
          };
        }
      }
      return nestedArray;
    },
    stringToId: function(string) {
      //given 'x-y' will return ['x', 'y']
      let theId = string.split("-");
      //x && y will be turned into numbers, returns array
      let ID = theId.map(stringNumber => parseInt(stringNumber));
      return ID;
    },
    rightClick: function(cell) {
      //let Id = this.stringToId(cell.target.id);

      let cellClicked = this.getCell(cell.target.id);

      if (this.firstClick) {
        //this.fixFirstClickBomb(cell.target.id);
        this.firstClick = false;

        this.allNearbyCells(cellClicked.id, cell => {
          console.log(cell.id);
        });
      }

      if (cellClicked.hasbomb) {
        alert("youve clicked a bomb, reload page to play again");
      }
    },
    fixFirstClickBomb: function() {
      //let bombsRemoved = [];
    },
    removeBomb: function(cellId) {
      this.getCell(cellId, cell => {
        cell.hasBomb = false;
      });
    },
    getCell: function(cellID, fn) {
      //modify one cell
      let Id = this.stringToId(cellID);

      if (fn) {
        fn(this.board[Id[0]][Id[1]]);
      }
      return this.board[Id[0]][Id[1]];
    },
    allNearbyCells: function(cellId, fn) {
      let Id = this.stringToId(cellId);

      const x = Id[0];
      const y = Id[1];

      for (let i = x - 1; i <= x + 1; i++) {
        for (let j = y - 1; j <= y + 1; j++) {
          //if it dont exist, move on
          if (!this.board[i] || !this.board[i][j]) {
            continue;
          }
          //dont put the box you clicked on the array of ids that represent the surrounding ids
          if (i === x && j === y) {
            continue;
          }

          fn(this.board[i][j]);
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
td,
th {
  border: 1px solid black;
}
</style>>


