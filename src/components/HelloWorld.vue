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
      return Math.random() <= 0.2;
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
      for(let i = 0; i < bombsLocation.length; i++) {
        this.allNearbyCells(bombsLocation[i], nestedArray, (cell) => {cell.nearbyBombs++})
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

      //console.log(cellClicked.id)
      // this.getCell(cellClicked.id, (cell) => {
      //   cell.screen = 'M'
      // })
      let Id = this.stringToId(cellClicked.id);

      console.log(cellClicked.id, Id);

      //this.board[Id[0]][Id[1]].screen = 'W'

      // if (this.firstClick) {
      //   //this.fixFirstClickBomb(cell.target.id);
      //   this.firstClick = false;

      this.getCell(cellClicked.id, cell => {
        cell.screen = "M";
      });

      this.allNearbyCells(cellClicked.id, cell => {
        cell.screen = "M";
      });
      // }

      /*
        this.allNearbyCells(cellClicked.id, cell => {
          console.log(cell.id);
        });
        */

      if (cellClicked.hasbomb) {
        alert("youve clicked a bomb, reload page to play again");
      }
    },
    fixFirstClickBomb: function(cellId) {
      let bombsRemoved = [];

      this.getCell(cellId, cell => {
        if (cell.hasbomb) {
          cell.hasBomb = !cell.hasBomb;

          bombsRemoved.push(cell.id);
        }
      });

      //console.log(bombsRemoved)
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
        fn(this.board[Id[1]][Id[0]]);
      }
      return this.board[Id[1]][Id[0]];
    },
    allNearbyCells: function(cellId, nestedArray,  fn) {
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


