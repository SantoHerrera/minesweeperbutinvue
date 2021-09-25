<template>
  <div class="hello">
    <p>Bombs On Board {{bombsOnBoard}}</p>
    <!-- uncomment this for a cheat sheet
    <p>Bombs isMarked successfully {{bombsDiscoveredVerified}}</p>
    -->
    <table>
      <tbody>
        <tr v-for="(row,i) in board" :key="i">
          <td
            v-for="(cell, j) in row"
            :key="`${j}-${i}`"
            :id="`${j}-${i}`"
            v-on:click="cellClick"
            v-on:click.right="markCell"
          >{{ cell.screen }}</td>
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
      //bombsOnBoard: 0, these two gets set during f bombIncrement inside f createNestedArray,
      //bombsDiscoveredVerified: 0
      /*  this gives error/ vue warn. 
      if I set it here and try to increment it when f incrementBomb
      gets called it wont. so this is a little work around
      */
      shitLinked: "Goodluck!!",
      board: this.createNestedArray(9, 6),
      firstClick: true
    };
  },
  methods: {
    randomTrueFalse: function() {
      return Math.random() <= 0.2;
    },
    allNearbyCells: function(cellId, nestedArray, fn, skipMiddle) {
      //skipMiddle defaults to false, true applys function passed in to cellId
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
            if (skipMiddle) {
              continue;
            }
          }

          fn(nestedArray[i][j]);
        }
      }
    },
    changeCellScreen: function(id) {
      let cellClicked = this.getCell(id);

      if (cellClicked.hasBomb) {
        cellClicked.screen = "*";
      } else if (!cellClicked.hasBomb && cellClicked.nearbyBombs === 0) {
        cellClicked.screen = " ";
      } else {
        cellClicked.screen = `${cellClicked.nearbyBombs}`;
      }
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
    stringToId: function(string) {
      //given 'x-y' will return ['x', 'y']
      let theId = string.split("-");
      //x && y will be turned into numbers, returns array
      let ID = theId.map(stringNumber => parseInt(stringNumber));
      return ID;
    },
    //function dealing with board creation
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
            isMarked: false
          };
        }
      }
      this.bombIncrement(bombsLocation, nestedArray);

      return nestedArray;
    },
    bombIncrement: function(arr, nestedArray) {
      this.bombsOnBoard = arr.length;
      this.bombsDiscoveredVerified = arr.length;
      //given arrayy of the ids of bombs, it will increement allnearbycell, skipping middle
      for (let i = 0; i < arr.length; i++) {
        this.allNearbyCells(
          arr[i],
          nestedArray,
          cell => {
            cell.nearbyBombs++;
          },
          true
        );
      }
    },
    //function when you click board
    clear3by3OfBombs: function(cellId) {
      this.allNearbyCells(cellId, this.board, cell => {
        if (cell.hasBomb) {
          cell.hasBomb = false;

          this.bombsOnBoard--;
          this.bombsDiscoveredVerified--;

          this.allNearbyCells(
            cell.id,
            this.board,
            cell => {
              cell.nearbyBombs--;
            },
            true
          );
        }
      });
    },
    cellClick: function(cell) {
      let cellClicked = this.getCell(cell.target.id);

      if (this.firstClick) {
        this.firstClick = false;
        this.clear3by3OfBombs(cellClicked.id);
        //changes screeen of 3 by 3
        this.allNearbyCells(cellClicked.id, this.board, cell => {
          this.changeCellScreen(cell.id);
        });
      }
      //when you decide to click on a cell that was marked, forces you to unmark cell before being able to click it
      if (cellClicked.isMarked) {
        return;
      }

      if (cellClicked.hasBomb) {
        alert("youve clicked a bomb, reload page to play again");
      }

      cellClicked.hasBeenClicked = true;

      this.changeCellScreen(cellClicked.id);
    },
    markCell: function(e) {
      e.preventDefault();

      let cellClicked = this.getCell(e.target.id);

      if (cellClicked.hasBeenClicked) {
        return;
      }

      cellClicked.isMarked = !cellClicked.isMarked;

      if (cellClicked.isMarked) {
        //mark cell
        cellClicked.screen = "M";
        this.bombsOnBoard--;
        if (cellClicked.hasBomb) {
          this.bombsDiscoveredVerified--;
        }
      } else {
        //unmark cell
        cellClicked.screen = "?";
        this.bombsOnBoard++;
        if (cellClicked.hasBomb) {
          this.bombsDiscoveredVerified++;
        }
      }
      this.checkWin();
    },
    checkWin: function() {
      if (this.bombsDiscoveredVerified === 0) {
        alert('you"ve won hoe');
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


