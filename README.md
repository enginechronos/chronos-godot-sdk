
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

```bash
res://addons/chronos/
````

Files:

```text
Chronos.gd
ChronosRESTClient.gd
ChronosSSEClient.gd
ChronosTypes.gd
plugin.gd
plugin.cfg
```

---

## Enable the Plugin

* Open: **Project → Project Settings → Plugins**
* Find **Chronos**
* Set to **Enabled**

---

## Configure Chronos

```gdscript
Chronos.configure(
  "https://YOUR-VERCEL-URL",
  "CHRONOS_API_KEY",
  "your_world_id",
  "npc_id"
)

# Plug-and-play runtime
Chronos.configure_runtime(true, 2, 50)

Chronos.start()
```

---

## Recommended SDK Flow (0.1v)

Your game sends events → Chronos stores memory → Brain derives NPC state → your game reacts.

---

## Important Call 1 — Listen for NPC State Updates

When Chronos updates an NPC’s state, your game listens and reacts:

```gdscript
Chronos.npc_state_updated.connect(_on_npc_state_updated)
```

### Example handler

```gdscript
 # Example handler for NPC updates

func _on_npc_state_updated(row):

    var npc_id = row["npc_id"]
    var state = row["state"]

    print("NPC state updated:", npc_id, state)
    
    
    # Example in a real game:
    func _on_npc_state_updated(row):

    var state = row["state"]

    if state["mood"] == "hostile":
        guard_attack_player()

    if state["mood"] == "friendly":
        guard_allow_entry()
```

---

## Important Call 2 — Send Gameplay Events

When something important happens in your game, send it to Chronos:

```gdscript
Chronos.append_event(
  "player_1",
  event_type,
  payload,
  true
)
```

### Example

```gdscript
Chronos.append_event(
  "player_1",
  "player_lied_to_guard",
  {"context": "conversation"},
  true
)
```

Chronos will automatically:

* Store the event
* Run the Brain
* Update NPC state
* Push the update back to the game

---

## Optional Call — Load Saved NPC State

```gdscript
Chronos.get_npc_state("guard_1")
```

Ensures the NPC reflects saved memory immediately.

---

## Example Project

Full demo:
[https://github.com/enginechronos/chronos-demo](https://github.com/enginechronos/chronos-demo)

---

## Docs

[https://chronos-magic-engine-live.vercel.app/docs](https://chronos-magic-engine-live.vercel.app/docs)

---

# Community

Building with Chronos?  
Have questions, feedback, or ideas?

Join the community or reach out directly.

### Discord (Developer Community)
[ https://discord.gg/Pg6Txu8YyB ]

### Chronos Updates (Project X)
[ https://x.com/EngineChronos ]

### Founder Contact
[ https://x.com/mr_manasmishra ]

---

## License

MIT License


