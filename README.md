# Lobby Example

This is an example of a lobby for Multisynq games, implemented with Multisynq. It uses the game from step 9 of the [Multiblaster Tutorial](https://multisynq.io/docs/client/tutorial-1_6_multiblaster.html).

## TL;DR

To use this lobby in your own game:

1. rename your own game's main HTML file to `game.html`
2. copy the lobby's `index.html` next to your game's `game.html`
3. in that `index.html`, edit the `Session.join()` arguments (e.g. your API key, appId etc)
4. edit your own `game.html` using code from the lobby's `game.html`
   - add the `LobbyRelayModel` and `LobbyRelayView` classes
   - add the `joinSessionFromLobby()` function (replacing your old `Session.join()`, but adjust the arguments like API key etc)
   - in your game's root model `init()` method, add

         LobbyRelayModel.create()

   - in your game's root view `constructor()`, add

         this.lobbyRelay = new LobbyRelayView(this.wellKnownModel("lobbyRelay"));

   - in your game's root view `detach()` method, add

         this.lobbyRelay.detach();
         this.lobbyRelay = null;

That should do it. Launch `index.html` in two browser windows, click the "Host New Game" button, enter a session name, and your game should start. The lobby in the other window should show the game session and allow you to join it.

## How it works

The lobby has a Multisynq session that keeps track of players coming in, and sessions going on. It shows them in a list.

When a player joins a session, the game is loaded in an iframe fully covering the lobby itself, but the lobby session stays active in addition to the game session in the iframe. The game sends updates to the lobby via in-browser messages. That's how the lobby knows which sessions are active.

For each game session, all but one players leave the lobby session. One player acts as relay. This is so the number of users in the lobby session doesn't get too large.

