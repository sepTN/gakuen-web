---
title: "Online Visual Novel in Godot: Case Study on Sentou Gakuen"
summary: "In this short case study, we will focus on how Sentou Gakuen, an online visual novel developed in Godot Engine, attempts to redefine the genre. We'll explore some of the challenges involved and why this approach was chosen for the project. Let's dive in!"
tags: ["post","blog"]
#externalUrl: ""
#showSummary: true
date: 2024-08-17
draft: false
---

When you talk about visual novel games, you'd normally think of a static story-driven experience, often with branching paths and multiple endings. You may have hundreds of routes but ultimately designed for solo play. It is why some people consider that visual novel as a genre isn't a game, but more of an interactive storybook. 
{{< typeit speed=22 >}}However, what if you could take the concept of a visual novel and turn it into an online interactive experience?{{< /typeit >}}

{{< youtube Bfsm5jRWZGk >}}

In this short case study, we will focus on how Sentou Gakuen, an online visual novel developed in Godot Engine, attempts to redefine the genre. We'll explore some of the challenges involved and why this approach was chosen for the project. Let's dive in!

### Traditional Visual Novels
Visual novels are well-established in game development—often comprising rich narratives, multiple routes, and player choices that influence outcomes. There are various engine options for creating visual novels, such as Ren'Py, TyranoBuilder, Unity, and Godot Engine is no different. With Godot Engine, numerous developers have created compelling and visually stunning visual novels that allow players to dive deep into story-driven experiences. However, when we introduce the concept of *online interactivity* into a visual novel, a whole new set of challenges emerges.

### Gameplay: Dynamic vs. Static
Traditional visual novels are primarily static, designed around branching paths and predefined outcomes. They typically consist of:

- **Linear Narratives**: While choices may lead to different routes, all possibilities are predetermined. Time, place, and location depends on how are you progressing through the story.
- **Single-Player Focus**: The game is designed for one person to play at their own pace, with no impact from other players. Players are limited to the choices provided by the game, with no real-time interactions or shared experiences.

In contrast, Sentou Gakuen introduces dynamic mechanics:
- **Shared World**: Player actions influence not just their experience, but the experience of others, similar to a multiplayer RPG. The world has day and night cycles, when it's morning for one player, it's morning for everyone. 
- **Community Interaction**: Players can communicate through an in-game chat system, post on community boards, and engage in shared events.

This kind of shared experience requires a different design approach. The narrative isn't purely about routes but rather a fluid storyline influenced by the cumulative actions of many players.

### Interactive Visual Novels
This paradigm shift from a static, single-player experience to a dynamic, shared world introduces a host of technical and design challenges such as, but not limited to:

- **A Persistent World**: In a multiplayer setting, ensuring that all players experience the same world state at the same time is crucial, preventing discrepancies that could disrupt immersion.
- **Keeping the World Alive**: Creating a lively and dynamic environment, ensuring the game world feels active and interconnected, even when players are exploring alone.
- **Communication**: Effectively managing player communication in a way that aligns with the visual novel's narrative style while accommodating simultaneous interactions.
- **Player versus Player (PvP)**: Integrating competitive elements into a visual novel in a way that complements the narrative-driven experience rather than overshadowing it.

Let’s delve into how the game tackles these obstacles to redefine the genre.

### Key Challenge: A Persistent World
In traditional offline visual novels, time and location are tightly controlled by the player’s individual progression through the story. The *world revolves around the player*, and events occur based solely on where they are in the narrative. However, Sentou Gakuen presents a dynamic world where multiple players interact in real time. This shift from static storytelling to a shared world creates a unique challenge: making sure that all players experience the same world state at the same time.

#### The Problem: Synchronization in a Dynamic World
In Sentou Gakuen, the world needs to be consistent for everyone. For example, if it’s morning for one player, it should be morning for everyone else in the game. Similarly, if a bank is open for business, it has to be open for every player, not just those at a particular point in the story. Having players exist in different “times” would break immersion and create confusion. This required a solution to keep everyone on the same page regarding the state of the game world.

{{< figure
src="day_night_cycle_in_sentou_gakuen.gif"
alt="Day-Night Cycle in Sentou Gakuen"
caption="A day-night cycle that is synchronized for all players."
>}}

#### The Solution: Real-Time Synchronization Using Unix Timestamps
To address this challenge, a simple yet effective solution was implemented: Shared Timestamps. By using a common time reference, the game ensures that all players experience the same time of day, the same events, and the same world state. This simple mechanism keeps the world persistent and coherent, even as multiple players interact with it simultaneously.

### Key Challenge: Keeping the World Alive
What makes a game world feel alive? It’s the sense that things are happening, even when you’re not around. This is not a concern in single-player games, where the world revolves around the player. But in a shared online environment, that's not the case.

#### The Problem: Making it Feel Lively
In a multiplayer game, one of the challenges is ensuring the world feels alive and dynamic. Players need to feel like they are part of an active, bustling world, *even if there are no other players around* at the same time. A static world can quickly become boring, leaving players feeling disconnected from the community.

#### The Solution: The World Full of Happenings
To make Sentou Gakuen feel lively, each zone in the game features an interface that logs recent events and activities that other players have completed. While not updated in real time, these logs show what players have done in that zone, such as someone being defeated in a brawl or completing a particular event. By displaying these “happenings,” the game creates a sense of a constantly evolving world, making players feel part of a larger, active community. This feature not only adds immersion but also encourages players to engage with the world, knowing their actions contribute to its evolving story.

{{< figure
src="zone_happenings_in_sentou_gakuen.jpg"
alt="Zone Happenings in Sentou Gakuen"
caption="Each zone in Sentou Gakuen features an interface that logs recent events and activities that other players have completed."
>}}

Furthermore, the faction system enhances the dynamism of the game world. Players can join one of two factions: *Hikari* or *Yami*. Zones in the game have a "controlling faction," determined by which faction has the most presence in that zone. By simply being online in a particular zone, players contribute to their faction’s influence. For example, if a player from Yami is in the School Gate zone, they increase Yami's influence there.

{{< figure
src="factions_in_sentou_gakuen.jpg"
alt="Factions in Sentou Gakuen"
caption="The dominant faction in a zone rewards its members with buff, encouraging faction members to compete for control of different zones."
>}}

The dominant faction in a zone rewards its members with buff, encouraging faction members to compete for control of different zones. This ongoing power struggle ensures the game world remains dynamic, with faction control shifting based on player participation, creating an evolving landscape for players to interact with.

### Key Challenge: Chat System Integration
One of the biggest challenges in creating an online visual novel is handling player communication in a way that fits the traditional visual novel style. In a normal visual novel, character dialogues are presented with neatly formatted text boxes that pop up one at a time, allowing players to fully absorb the conversation at their own pace. Initially, The project aimed for a similar experience, where player chat messages would appear as dialogues, just like when a character speaks in the game. However, this approach quickly ran into significant issues.

{{< figure
src="player_chatting.gif"
alt="Player Chatting in Sentou Gakuen"
caption="Players can communicate through an in-game chat system."
>}}

#### The Problem: Managing Concurrent Chats
In Sentou Gakuen, multiple players are interacting with each other simultaneously, and when there's a lot of chatter happening at once, it can be challenging to keep up with the conversation. The traditional visual novel dialogue box system, while great for presenting character dialogues, wasn't well-suited for handling real-time player interactions. Players could easily miss messages or get lost in the conversation, leading to a disjointed experience.

#### The Solution: Transition to a "Chat Box"
To solve this problem, a decision to add seamless transition from visual novel-style dialogue boxes to a more conventional MMO chat box system was made. This allowed players to keep up with ongoing conversations more naturally, while still being able to engage with the story and interact with other players. The chat box will replace the dialogue box automatically when there's too much chatter happening in the game, and players can switch between the two as needed.

{{< figure
src="chat_box_in_sentou_gakuen.png"
alt="Chat Box in Sentou Gakuen"
caption="Traditional chatbox allows players to keep up with ongoing conversations more naturally."
>}}

This compromise maintained the essence of a visual novel while accommodating the realities of multiplayer communication. Players could enjoy the rich story elements without losing track of real-time conversations happening within the game.

### Key Challenge: Player versus Player
How do you introduce competitive elements into a visual novel without compromising the experience? PvP mechanics can add depth and excitement to a game, but in a genre focused on storytelling, it’s essential to find a balance that keeps the narrative at the forefront while still providing engaging gameplay.

#### The Problem: Can this be done?
Integrating Player versus Player (PvP) mechanics into a visual novel is a concept that has rarely, if ever, been explored. Most visual novels focus on narrative-driven experiences, often limiting player interactions to choices and branching paths. Adding competitive elements introduces a challenge: how can we incorporate PvP in a way that feels natural and maintains the essence of a visual novel, rather than turning it into a traditional combat game?

{{< figure
src="pvp_in_sentou_gakuen.jpg"
alt="PvP in Sentou Gakuen"
caption="Sentou Gakuen introduces indirect, asynchronous PvP that fosters competition in two key ways."
>}}

#### The Solution: Asynchronous PvP
While Sentou Gakuen doesn't feature direct real-time PvP, it introduces indirect, asynchronous PvP that still fosters competition in two key ways:

{{< figure
src="fight_club_in_sentou_gakuen.jpg"
alt="Fight Club in Sentou Gakuen"
caption="Players can create their own loadout and participate as challengers in the Fight Club."
>}}

- Players can create their own loadout and participate as challengers. Once enrolled, other students can challenge these pre-set loadouts in turn-based combat. This system allows for strategic battles without the need for both players to be online simultaneously.
- In school zones, players have a chance to "appear" as a mob in another player’s instance during a fight. This simulates PvP encounters in a controlled environment and adds a personal touch to otherwise AI-controlled battles. If preferred, this feature can be turned off for those who want to avoid appearing in others' instances.


### Key Challenge: What Engine to Choose?
Godot Engine was chosen for Sentou Gakuen for several reasons:

{{< figure
src="godot_and_godotsteam.png"
alt="Godot Engine and GodotSteam"
caption="Godot Engine and GodotSteam were used to develop Sentou Gakuen."
>}}

- **Flexibility**: The engine’s open-source nature and robust feature set made it possible to implement complex systems and mechanics that would have been challenging in other engines.
- **Networking with GodotSteam**: The game utilizes GodotSteam, leveraging the Steamworks API to handle networking functionalities. This integration simplifies the implementation of multiplayer mechanics and ensures a seamless online experience.
- **Ease of Use**: Godot’s GDScript made it easy to prototype and iterate on game systems, allowing for rapid development and testing of new features.
- **Cross-Platform Compatibility**: Godot’s support for multiple platforms ensures that the project can reach a wide audience, regardless of the system they’re running.

And while Godot Engine was the choice for the project, it’s essential to evaluate the engine based on the specific needs of your project. Each engine has its strengths and weaknesses, and choosing the right one can significantly impact the development process and the final product.

### Wrappping Up
Creating Sentou Gakuen in Godot was a journey of pushing the boundaries of what a visual novel can be. While traditional visual novels focus on static routes and single-player experiences, Sentou Gakuen aimed to create a shared, interactive story space where every player’s actions could influence the wider game world. Godot’s flexibility allowed the project to achieve that vision, even though it came with challenges.

This case study highlights possibilities for future visual novel projects, showing that the genre can evolve beyond its traditional roots to create engaging, dynamic experiences that bring players together in new and exciting ways. Visual novels can become more than just interactive storybooks!

<iframe src="https://store.steampowered.com/widget/405680/?utm_source=css" frameborder="0" width="100%" height="190"></iframe>

If you’re interested in exploring the world of Sentou Gakuen, you can check out the game on [Steam here](https://store.steampowered.com/app/405680/Sentou_Gakuen_Revival/?snr=1_5_1100__1100&utm_source=css) or join the [Discord](https://discord.gg/GUJkUWymmT), while the final release date is yet to be announced, in the meantime you can try the demo to get a taste of what’s to come.

<hr>
{{< alert "twitter" >}}
Don't forget to [follow](https://twitter.com/sentougakuen) on Twitter.
{{< /alert >}}

<small>You're welcome to share or reference this article! If you'd like to repost it, please include a link back to the original post or use a canonical meta tag to ensure proper attribution and help the content connected to its source.</small>
