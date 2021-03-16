<script>
  import Player from "./Player.svelte";
  import GameInfoPanel from "./GameInfoPanel.svelte";
  import * as S from "svelte-materialify";
  import Snackbar from "./Snackbar.svelte";

  let players = [
    {
      id: 0,
      name: "Rafael",
      age: "34",
      score: 0,
      country: "Spain",
      status: "pending",
      disabled: false,
      avatar:
        "http://t0.gstatic.com/images?q=tbn:ANd9GcS302112Ltqg1w2lA_Pa0IMBo7_y0S3oF8jAC16oYP3sFWqfNL7YzwB8w1ehl6D",
    },
    {
      id: 1,
      name: "Andy",
      age: "33",
      score: 0,
      country: "UK",
      disabled: false,
      status: "pending",
      avatar:
        "https://stoneforest.ru/wp-content/uploads/2018/06/andy-murray-1.jpg",
    },
  ];
  let games = 0;
  $: showSnackbar = false;
  let snackText = "";
  let totalGames = {
    player1: players[0].name,
    player2: players[1].name,
    game1: { player1: "x", player2: "x" },
    game2: { player1: "x", player2: "x" },
    game3: { player1: "x", player2: "x" },
  };

  let initial = {
    player1: players[0].name,
    player2: players[1].name,
    game1: { player1: "x", player2: "x" },
    game2: { player1: "x", player2: "x" },
    game3: { player1: "x", player2: "x" },
    players: [
      {
        id: 0,
        name: "Rafael",
        age: "34",
        score: 0,
        country: "Spain",
        status: "pending",
        disabled: false,
        avatar:
          "http://t0.gstatic.com/images?q=tbn:ANd9GcS302112Ltqg1w2lA_Pa0IMBo7_y0S3oF8jAC16oYP3sFWqfNL7YzwB8w1ehl6D",
      },
      {
        id: 1,
        name: "Andy",
        age: "33",
        score: 0,
        country: "UK",
        disabled: false,
        status: "pending",
        avatar:
          "https://stoneforest.ru/wp-content/uploads/2018/06/andy-murray-1.jpg",
      },
    ],
  };
  let scores = [0, 15, 30, 40];
  let quantityHits = [2, 5, 6, 9, 8, 7, 10, 3];

  let currnetRandomHitsQuantity =
    quantityHits[Math.floor(Math.random() * quantityHits.length)];

  let quantityHitsCounter = 0;

  function getRandomScore() {
    return scores[Math.floor(Math.random() * scores.length)];
  }

  function getPlayerIndex(name) {
    return players.findIndex((obj) => obj.name === name);
  }

  function toggleSnackbar() {
    showSnackbar = true;
    setTimeout(() => {
      showSnackbar = false;
    }, 2000);
  }

  function getOpponentIndex(playerIndex) {
    let opponentIndex = 1;
    if (playerIndex === 1) opponentIndex = 0;
    return opponentIndex;
  }

  function showWinner() {
    let firstPlayer =
      totalGames.game1.player1 +
      totalGames.game2.player1 +
      totalGames.game3.player1;
    let secondPlayer =
      totalGames.game1.player2 +
      totalGames.game2.player2 +
      totalGames.game3.player2;
    let winnerTotalScore;
    let winnerName;
    snackText = `Game Winned by ${winnerTotalScore} with total score ${winnerName}`;
    if (firstPlayer > secondPlayer) {
      winnerTotalScore = firstPlayer;
      winnerName = totalGames.player1;
      players = players.filter((player) => {
        return player.name !== winnerName;
      });
      return toggleSnackbar();
    }
    if (secondPlayer > firstPlayer) {
      winnerTotalScore = secondPlayer;
      winnerName = totalGames.player2;
      players = players.filter((player) => {
        return player.name !== winnerName;
      });
      return toggleSnackbar();
    }
    snackText = "Friendship won!";
    return toggleSnackbar();
  }

  function completeCurrentSet(detail) {
    let score = getRandomScore();
    let playerIndex = getPlayerIndex(detail);
    quantityHitsCounter = 0;
    games = ++games;
    toggleSnackbar();
    snackText = `Set${games} been winned by ${players[playerIndex].name} with score ${score}`;
    totalGames[`game${games}`][`player${++playerIndex}`] = score;
    currnetRandomHitsQuantity =
      quantityHits[Math.floor(Math.random() * quantityHits.length)];
    if (games === 3) {
      showWinner();
      setTimeout(() => {
        setNewGame();
      }, 6000);
    }
    // players[playerIndex].score = players[playerIndex].score + score;
  }

  function submissionHandler(event) {
    quantityHitsCounter = ++quantityHitsCounter;
    if (currnetRandomHitsQuantity === quantityHitsCounter)
      completeCurrentSet(event.detail);

    let playerIndex = getPlayerIndex(event.detail);
    let opponentIndex = getOpponentIndex(getPlayerIndex(event.detail));
    if (players[opponentIndex]) {
      players[opponentIndex].disabled = false;
    }
    players[playerIndex].status = "playing";
    players[playerIndex].disabled = true;
    setTimeout(() => {
      players[playerIndex].status = "pending";
    }, 1000);
  }

  function setNewGame() {
    const newState = players.map((obj) => {
      return { ...obj, status: "pending", score: 0, disabled: false };
    });
    totalGames = initial;
    players = newState;
    quantityHitsCounter = 0;
    games = 0;
  }
</script>

<main>
  <div class="game-wrapper">
    <Snackbar {snackText} {showSnackbar} />
    <GameInfoPanel {totalGames} />

    <div class="players-wrapper" >
      {#each players as player, i}
        <Player index={i} on:submission={submissionHandler} {...player} />
      {/each}
    </div>
    <S.Button on:click={setNewGame} class="new-game-btn">New game</S.Button>
  </div>
</main>

<style>
  .players-wrapper {
    display: flex;
  }
  .new-game-btn {
    width: 300px;
    margin-top: 20px;
    border-radius: 10px;
    cursor: pointer;
  }

  .game-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-direction: column;
  }
</style>
