![Logo](https://blueva.net/uploads/post_images/1-1679771276.png "BlueChunk Logo")

<div align="center">
  <a href="https://ko-fi.com/bluevanet"><img src="https://raw.githubusercontent.com/intergrav/devins-badges/v3/assets/cozy/donate/kofi-plural_64h.png" alt="Donate"></a>
  <a href="https://blueva.net/wiki/bluechunk"><img src="https://github.com/intergrav/devins-badges/blob/v3/assets/cozy/documentation/readthedocs_64h.png?raw=true" alt="Documentation"></a>
  <a href="https://discord.gg/kgxBr8pwpU"><img src="https://github.com/intergrav/devins-badges/blob/v3/assets/cozy/social/discord-plural_64h.png?raw=true" alt="Discord"></a>
</div>

## Why Use BlueChunk?
BlueChunk is a chunk loading optimizer built specifically for Paper and its forks. It reduces the lag spikes that happen when players explore new terrain, teleport, or log in. It does this by moving chunk work off the main thread, spreading it out over time, and scaling it to what your server can handle.

BlueChunk does not replace or patch Paper's internals. It builds on Paper's own async chunk system and API, so it stays compatible across updates and works alongside your other plugins.

⚠️ Warning: This plugin is still a work in progress and is not recommended for use on public servers. ⚠️

## Features
### Already Added
- **Smart Pre-Generation:** Generates your world ahead of time, inside the world border or a set radius. It pauses automatically when players are online or MSPT gets too high.
- **Async Teleport Prewarming:** Loads the chunks at a teleport destination in the background before the player arrives, so warps, homes, and RTP don't freeze the server.
- **Dynamic View Distance:** Lowers or raises each player's view and simulation distance based on server load, then restores it when things calm down.
- **Hot Chunk Retention:** Keeps busy areas like spawn, shops, and hubs loaded with plugin chunk tickets. This stops those areas from repeatedly loading and unloading.
- **Idle Chunk Cleanup:** Releases chunks that nobody has been near for a while, which frees memory without hurting active areas.
- **Sync Load Detection:** Warns you when another plugin forces a chunk to load on the main thread, and tells you which plugin did it.

### Planned Features
- Per-world profiles with separate settings for each world.
- A live dashboard showing chunk load times, generation rate, and pre-gen progress.
- Region-based pre-gen scheduling for off-peak hours.
- Integration with Chunky, so existing pre-gen jobs carry over.

And more to come!

## Why It Works
**Generating a chunk costs far more than loading one.** A brand-new chunk needs terrain noise, biomes, structures, features, and lighting calculations. A chunk that's already saved only needs to be read from disk and decompressed. Most exploration lag comes from generation. Pre-generating converts that expensive work into cheap disk reads, and it happens while nobody is playing.

**Paper already has an async chunk system, and BlueChunk makes sure it gets used.** Paper can load and generate chunks off the main thread. However, anything that asks for a chunk synchronously (`getChunkAt`, regular teleports, some plugins) makes the main tick wait for it. BlueChunk routes its own work through Paper's async API (`getChunkAtAsync`, `teleportAsync`). It also flags other plugins that request chunks synchronously, so you can find the real source of your lag.

**Throttling based on MSPT keeps the tick loop healthy.** A server has 50 ms per tick to stay at 20 TPS. BlueChunk watches MSPT and slows down or pauses its own work before your players would notice. Background tasks never compete with gameplay.

**View distance grows quadratically.** A player with view distance `r` keeps roughly `(2r + 1)²` chunks around them. At a distance of 10 that's 441 chunks per player; at 8 it's 289, about a third fewer. When the server is under heavy load, trimming a couple of chunks of distance gives a large saving for a small visual cost. Distance goes back up once the load drops.

**Chunk tickets prevent thrashing.** Without tickets, a chunk at a busy spawn can unload the moment a player steps away and then reload seconds later when someone else arrives. Holding a ticket on high-traffic areas turns those repeated load/unload cycles into a single load.

## Requirements
- Paper (or a fork such as Purpur) 1.21+
- Java 21

## Compiling from Source

BlueChunk is an open source project and the source code is available on [GitHub](https://github.com/BluevaDevelopment/BlueChunk). To build the plugin from source, you will need [Java Development Kit (JDK) 21](https://adoptium.net/) installed on your machine.

With an IDE like IntelliJ IDEA or Eclipse, you can import the source code and build the plugin using Maven. Here are the steps to build BlueChunk using IntelliJ IDEA:

1. Download and install IntelliJ IDEA on your computer.
2. Clone the BlueChunk repository to your local machine.
3. Open IntelliJ IDEA and click on "Open" or "Import Project".
4. Navigate to the location where you cloned the BlueChunk repository.
5. Choose "Import project from external model" and then "Maven".
6. Follow the instructions on the screen and configure any additional options as needed.
7. Click on "Finish" to import the project.
8. Once imported, open the "pom.xml" file to verify that all dependencies are resolved correctly.
9. Compile the project by clicking on "Build".

## Support BlueChunk
BlueChunk is maintained by developers in their spare time. If you enjoy using BlueChunk and would like to support its development, consider [making a donation](https://ko-fi.com/bluevanet). Your support will help us continue to improve and maintain this project.

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/V7V3IE7VS)
