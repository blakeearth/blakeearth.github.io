# 🌱 Block Placement and Removal in Homegrown

[Homegrown](https://blake.earth/tag:Homegrown) is a W.I.P. [open-source game](https://github.com/blakeearth/homegrown) I've been working on off-and-on for about a year. It's a multiplayer game in which players grow and build up their own small islands, accepting other players as visitors (and collaborators, should owners extend permission). Players can open up shops to trade their goods and invite N.P.C.s to their islands who trade with players and interact with the island. Players can also add behaviors to animate their in-game models and make them interactive.

Rarely do motivation and free time align, so I'm taking development slowly but surely. A lot of the groundwork for these systems is already laid. As of now, I have accounts management, server connections, models (i.e. block placement, removal and selection), syncing of models, syncing of multiplayer player movements, and world and inventory saving. Frustratingly, not much of this is worth showing off to friends and family; it's hard to see the work I've done on the surface level. The game isn't a game yet but a collection of conversing, interactive systems.

The most recent completed implementation is block placement and removal. I don't claim that my method of doing this is the best, but it seems to work fairly well (and remains fast at relatively large scale). Here's how that looks (roughly):

1. Models (essentially collections of blocks) have three layers of tiles: one to be viewed in front of the player (without collision), one to be rendered alongside the player (with collisions), and one behind the player (without collisions). Models have a method to place blocks that preliminarily verifies the placement and then request the placement server-side. For speed reasons, the block is placed client-side immediately. Each time a new block is placed, the model recalculates its outline.
2. If the placement is successful, the server-side representation of the model reserves that block and layer world-/island-wide so other models can't place blocks there and sends a message to all other clients telling them to place a block there. If verification is not successful, we tell the requesting client to remove the block. Verification (mainly) consists of verifying the item is in the player's inventory, checking that there is an adjacent block (world-wide), and checking that the requesting client has the right permissions. There's a similar verification process for removal of blocks.
3. Models are selected for editing by hovering over them with a mouse. The world gets the mouse position, checks if the mouse's position aligns with a reserved block coordinate, and if it does, tells that model to display its outline. If the player clicks the model, they begin editing it. If they right-click, the world calls the edited model's place block method. Similarly, clicking removes the hovered block.

Models, like worlds, are saved server-side only and transmitted to players as needed.

If you're interested in future posts about my game development process and other topics, subscribe via email and get them right in your inbox.

<!--emailsub-->
You're also welcome to subscribe via R.S.S. or follow the development on the Fediverse (@house-mangrove@write.as).

Up next, I'm working on the G.U.I. for editing model properties and making servers/accounts easier to save in-game.

#Homegrown #Gamedev