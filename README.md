![Chronos Engine](images/chronos-banner.png)

# Chronos Godot SDK

Official Godot SDK for Chronos Engine.

Chronos gives your game **persistent world memory, evolving NPC state, and AI-driven behavior**.

Instead of NPCs forgetting everything between sessions, Chronos lets them **remember player actions and react over time**.

---

## Supported Versions

- Godot 3.6
- Godot 4.5

---

## Installation

Copy the SDK into your project:


res://addons/chronos/


Files:


Chronos.gd
ChronosRESTClient.gd
ChronosSSEClient.gd
ChronosTypes.gd
plugin.gd
plugin.cfg


---

## Enable the Plugin

- Open: Project → Project Settings → Plugins  
- Find **Chronos**  
- Set to **Enabled**

---

## Configure Chronos

```gdscript
Chronos.configure(
  "https://YOUR-VERCEL-URL",
  "CHRONOS_API_KEY",
  "your_world_id",
  "npc_id"
)

Chronos.configure_runtime(true, 2, 50)
Chronos.start()

```

Recommended SDK Flow (MVP)

Your game sends events → Chronos stores memory → Brain derives NPC state → your game reacts.

Important Call 1 — Listen for NPC state updates

When Chronos updates an NPC’s state, your game listens for the update and reacts to the new behavior.

Chronos.npc_state_updated.connect(_on_npc_state_updated)

# Example handler for NPC updates
func _on_npc_state_updated(row):

  var npc_id = row["npc_id"]
  var state = row["state"]

  print("NPC state updated:", npc_id, state)
Important Call 2 — Send gameplay events

When something important happens in your game, send it to Chronos.

Chronos.append_event(
  "player_1",
  event_type,
  payload,
  true
)

Example:

Chronos.append_event(
  "player_1",
  "player_lied_to_guard",
  {"context":"conversation"},
  true
)

Chronos will automatically:

store the event

run the Brain

update NPC state

push the update back to the game

Optional Call — Load saved NPC state on startup
Chronos.get_npc_state("guard_1")

This ensures the NPC reflects saved memory immediately.

Example Project

Full demo:

https://github.com/enginechronos/chronos-demo

Docs

https://chronos-magic-engine-live.vercel.app/docs

License

MIT License