<script>
  import Chessboard from "chessboardjs";
  import { Chess } from "chess.js";
  export let selectedLine;

  const { playerMoves, player, computerRegex, computerMoves } = selectedLine;

  const game = new Chess();
  let moveCount = 0;

  setTimeout(() => {
    function onSnapEnd() {
      board.position(game.fen());
    }

    let config = {
      orientation: player,
      draggable: true,
      position: "start",
      onDragStart: onDragStart,
      onDrop: onDrop,
      onSnapEnd: onSnapEnd,
    };

    let board = new Chessboard("board", config);

    updateStatus();

    if (player === "black" && moveCount === 0) {
      setTimeout(() => {
        game.move(computerMoves[0]);
        board.position(game.fen());
      }, 250);
    }

    function verifyPlayerMove(source, target) {
      let playerMove = source + " " + target;
      if (playerMove !== playerMoves[moveCount - 1]) {
        alert("not the right move, try again!");
        return false;
      } else {
        return true;
      }
    }

    function makeComputerMove(moveCount) {
      if (player === "white") {
        game.move(computerMoves[moveCount - 1]);
      } else {
        game.move(computerMoves[moveCount]);
      }

      board.position(game.fen());
    }

    function onDragStart(source, piece, position, orientation) {
      // do not pick up pieces if the game is over
      if (game.game_over()) return false;

      // only pick up pieces for the player
      if (piece.search(computerRegex) !== -1) return false;
    }

    function onDrop(source, target) {
      // see if the move is legal
      let move = game.move({
        from: source,
        to: target,
        promotion: "q", // NOTE: always promote to a queen for example simplicity
      });

      // illegal move
      if (move === null) return "snapback";

      moveCount++;
      updateStatus();

      verifyPlayerMove(source, target);
      window.setTimeout(makeComputerMove(moveCount), 500);
    }

    function updateStatus() {
      let status = "";

      let moveColor = "White";
      if (game.turn() === "b") {
        moveColor = "Black";
      }

      // checkmate?
      if (game.in_checkmate()) {
        status = "Game over, " + moveColor + " is in checkmate.";
      }

      // draw?
      else if (game.in_draw()) {
        status = "Game over, drawn position";
      }

      // game still on
      else {
        status = moveColor + " to move";

        // check?
        if (game.in_check()) {
          status += ", " + moveColor + " is in check";
        }
      }
    }
  }, 0);
</script>

<div id="board" />

<style>
  #board {
    width: 30rem;
    height: 30rem;
  }
</style>
