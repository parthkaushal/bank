<template>
  <div id="game-container">
  </div>
</template>

<script setup lang="ts">

import Phaser from "phaser";
import Boot from "~/game_assets/scenes/Boot.js";
import Preloader, { authEvents, AUTH } from "~/game_assets/scenes/Preloader.js";
import MainMenu, { STARTGAME } from "~/game_assets/scenes/MainMenu.js";
import MainGame from "~/game_assets/scenes/Game.js";


onMounted( async () => {
  console.log("mounted..")
  let game = null;
  if (!false) {
    const config = {
      type: Phaser.AUTO,
      gameTitle: "P2E Bank Panic | Phaser x Moralis",
      parent: "game-container",
      autoCenter: Phaser.Scale.CENTER_HORIZONTALLY,
      autoFocus: true,
      fps: {
        target: 60,
      },
      physics: {
        default: "arcade",
        arcade: {
          gravity: { y: 200 },
          debug: false,
        },
      },
      backgroundColor: "#282c34",
      scale: {
        mode: Phaser.Scale.ScaleModes.NONE,
      },
      scene: [Boot, Preloader, MainMenu, MainGame],
    };
    // init 2d game (Phaser canvas element)
    if (game === null) {
      // init instance of phaser game as per config
      game = new Phaser.Game(config);
      // listen to in-game events
      // before starting we sign in with wallet
      game.events.on("LOGIN_PLAYER", (event) => {
        console.log("⛓⛓⛓ Login via Web3 Wallet ⛓⛓⛓");
        // trigger wallet authentication
        login();
      });

      game.events.on("GAME_OVER", (event) => {
        console.log("Game Over: State Updated");
        /*
        // WIP: When GAME_OVER event triggered from within Phaser scene:  state checked for end states (win/lose).
        if (initState.gameId && initState.state === "win") {
          // 
          // set-up params
          const params = {
            gameId: initState.gameId,
            player: initState.player.address,
            winnings: initState.score,
          };
          win();
        }
        */
      });

      game.events.on("STARTGAME", (event) => {
        console.log("STARTGAME");
        // TODO: set initial game state
        // * gameId, etc
        // * run cloud function for admin bot to move funcs to  escrow
      });
    }
  }

});

</script>
