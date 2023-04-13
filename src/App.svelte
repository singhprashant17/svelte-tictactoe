<script lang="ts">
  import Board from "./lib/Board.svelte";
  import ControlButtons from "./lib/ControlButtons.svelte";

  let boardSize = 4;

  let state;
  var isXTurn: boolean;
  resetState();

  function onCellClicked(event) {
    let index = event.detail;
    if (state[index] != "") {
      return;
    }
    state[index] = isXTurn ? "X" : "O";
    isXTurn = !isXTurn;
    checkGameEnd();
  }

  function checkGameEnd() {
    const dimension = Math.sqrt(state.length);

    function areElementsSame(filterArray) {
      var first = filterArray[0];
      const allAreSame =
        first != "" &&
        filterArray.every(function (element) {
          return element === first;
        });
      if (allAreSame) {
        setTimeout(function () {
          alert(first + " wins");
        }, 100);
      }
      return allAreSame;
    }

    // check vertical array
    for (let i = 0; i < dimension; i++) {
      var filterArray = state.filter(function (currentValue, index) {
        return index % dimension == i;
      });
      if (areElementsSame(filterArray)) {
        return;
      }
    }

    // check horizontal array
    for (let i = 0; i < state.length; i += dimension) {
      const chunk = state.slice(i, i + dimension);
      filterArray = chunk;
      if (areElementsSame(filterArray)) {
        return;
      }
    }

    // check left diagonal
    filterArray = [];
    var pos = 0;
    while (pos < state.length) {
      filterArray.push(state[pos]);
      pos = pos + dimension + 1;
    }
    if (areElementsSame(filterArray)) {
      return;
    }

    // check right diagonal
    filterArray = [];
    var pos = dimension - 1;
    while (pos < state.length - 1) {
      filterArray.push(state[pos]);
      pos = pos + dimension - 1;
    }
    if (areElementsSame(filterArray)) {
      return;
    }
  }

  function resetState() {
    let totalBlocks = Math.pow(boardSize, 2);
    state = Array(totalBlocks).fill("");
    isXTurn = true;
  }
</script>

<main>
  <Board state="{state}" on:onCellClicked="{onCellClicked}" />
  <br />
  <ControlButtons on:reset="{resetState}" />
</main>
