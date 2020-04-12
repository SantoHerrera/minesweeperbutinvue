<template>
  <div class="hello">
    <table>
      <tbody>
        <tr v-for="(row,i) in board" :key="i">
          <td
            v-for="(cell, j) in row"
            :key="`${j}-${i}`"
            :id="`${j}-${i}`"
            v-on:click="rightClick"
          >{{ cell.screen }} {{cell.hasBomb }} {{cell.nearbyBombs}}</td>
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
      return Math.random() <= 0.4;
    },
    createNestedArray: function(x, y) {
      let nestedArray = [];

      let bombsLocation = [];
      for (let i = 0; i < y; i++) {
        nestedArray.push([]);
        for (let j = 0; j < x; j++) {
          //true represents bomb
          let isBomb = this.randomTrueFalse();

          if (isBomb) {
            bombsLocation.push(`${j}-${i}`);
          }
          //information of individual cell
          nestedArray[i][j] = {
            screen: "?",
            hasBomb: isBomb,
            hasBeenClicked: false,
            nearbyBombs: 0,
            id: `${j}-${i}`,
            wasrightClicked: false
          };
        }
      }
      for (let i = 0; i < bombsLocation.length; i++) {
        this.allNearbyCells(bombsLocation[i], nestedArray, cell => {
          cell.nearbyBombs++;
        });
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
      let cellClicked = this.getCell(cell.target.id);

      if (this.firstClick) {
        this.firstClick = false;

        this.clear3by3OfBombs(cellClicked.id);
      }

      if (cellClicked.hasbomb) {
        alert("youve clicked a bomb, reload page to play again");
      }
    },
    clear3by3OfBombs: function(cellId) {
      let bombsRemoved = [];
      //middle Cell, cell clicked id
      this.getCell(cellId, cell => {
        if (cell.hasbomb) {
          this.removeBomb(cell.id);

          bombsRemoved.push(cell.id);
        }
      });

      this.allNearbyCells(cellId, this.board, cell => {
        if (cell.hasBomb) {
          this.removeBomb(cell.id);

          bombsRemoved.push(cell.id);
        }
      });
    },
    removeBomb: function(cellId) {
      this.getCell(cellId, cell => {
        cell.hasBomb = false;
      });

      this.allNearbyCells(cellId, this.board, cell => {
        cell.nearbyBombs--;
      });
    },
    getCell: function(cellID, fn) {
      //modify one cell
      let Id = this.stringToId(cellID);
      //this shit kinda ugly
      if (fn) {
        fn(this.board[Id[1]][Id[0]]);
      }
      return this.board[Id[1]][Id[0]];
    },
    allNearbyCells: function(cellId, nestedArray, fn) {
      let Id = this.stringToId(cellId);

      const y = Id[0];
      const x = Id[1];

      for (let i = x - 1; i <= x + 1; i++) {
        for (let j = y - 1; j <= y + 1; j++) {
          //if it dont exist, move on
          if (!nestedArray[i] || !nestedArray[i][j]) {
            continue;
          }
          //skip the cell that was passedd in
          if (i === x && j === y) {
            continue;
          }

          fn(nestedArray[i][j]);
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


