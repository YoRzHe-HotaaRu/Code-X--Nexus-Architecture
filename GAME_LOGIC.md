# Game Logic

Documentation of core game systems and mechanics in Code[X]:Nexus.

---

## Player Controller

**Script**: `Scenes/Player/player.gd`  
**Base Class**: `CharacterBody2D`

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `speed` | float | Movement speed (default: 50.0) |
| `can_move` | bool | Movement lock flag |
| `last_direction` | Vector2 | Last movement direction for idle animation |

### Movement System

```gdscript
func _physics_process(delta):
    if not can_move:
        return
    
    var direction = Input.get_vector("move_left", "move_right", "move_up", "move_down")
    if direction != Vector2.ZERO:
        last_direction = direction
    
    velocity = direction * speed
    move_and_slide()
    update_animation()
```

### Animation States
- **Walking**: `walk_up`, `walk_down`, `walk_left`, `walk_right`
- **Idle**: `idle_up`, `idle_down`, `idle_left`, `idle_right`

### Signal Connections
Player freezes (`can_move = false`) during:
- Dialogue (`dialogue_started`)
- Chatbot open (`chatbot_opened`)
- Map open (`map_opened`)

---

## NPC Interaction System

### Interaction Area Pattern

All NPCs follow this structure:

```
StaticBody2D (NPC)
├── AnimatedSprite2D
├── CollisionShape2D
├── InteractionArea (Area2D)
│   └── CollisionShape2D
└── InteractionPrompt (Label)
```

### Interaction Logic

```gdscript
var player_in_range = null

func _on_interaction_area_body_entered(body):
    if body is Player:
        player_in_range = body
        interaction_prompt.show()

func _on_interaction_area_body_exited(body):
    if body is Player:
        player_in_range = null
        interaction_prompt.hide()

func _unhandled_input(event):
    if player_in_range and event.is_action_pressed("interact"):
        if not DialogueUI.is_active and not CompilerUI.is_visible():
            interact()
            get_tree().get_root().set_input_as_handled()
```

---

## Dialogue System

**Script**: `Scenes/Dialogue/dialogue_ui.gd`  
**Autoload**: `DialogueUI`

### Dialogue Data Structure

```gdscript
var dialogue_data = [
    {
        "character": "NPC Name",
        "portrait": preload("res://path/to/portrait.png"),
        "text": "Dialogue text here."
    },
    {
        "character": "NPC Name",
        "portrait": preload("res://path/to/portrait.png"),
        "text": "Question?",
        "choices": {
            "Choice A": { "branch": "branch_a", "signal_on_finish": "quest_id" },
            "Choice B": { "branch": "branch_b", "sfx": "wrong" }
        }
    }
]
```

### Choice Data Options

| Key | Type | Description |
|-----|------|-------------|
| `branch` | String | Next dialogue branch key |
| `signal` | String | Immediate signal emission |
| `signal_on_finish` | String | Signal after dialogue closes |
| `sfx` | String | "correct" or "wrong" sound |

### Flow

1. NPC calls `SignalManager.show_dialogue.emit(dialogue_data, branches)`
2. DialogueUI shows text line by line
3. Player advances with "interact" key
4. If choices present, buttons spawn
5. Choice triggers branch navigation or signal
6. On finish, `dialogue_ended` emitted

---

## Quest System

**Script**: `Scenes/Quest/quest_manager.gd`  
**Autoload**: `QuestManager`

### Data Structure

```gdscript
const DEFAULT_QUEST_DATA = {
    # World Start
    "met_aliff": false,
    "met_jackal": false,
    # ... 65+ objectives
}

var quest_data = {}
```

### Key Functions

```gdscript
# Mark objective as complete
func complete_objective(objective_id: String):
    if quest_data.has(objective_id) and not quest_data[objective_id]:
        quest_data[objective_id] = true
        quest_updated.emit()

# Check if world is fully complete
func are_world_objectives_complete(world_name: String) -> bool:
    var objectives_to_check = []
    match world_name:
        "world_start": objectives_to_check = WORLD_START_OBJECTIVES
        "world_1": objectives_to_check = WORLD_1_OBJECTIVES
        # etc.
    
    for objective_id in objectives_to_check:
        if not quest_data.get(objective_id, false):
            return false
    return true
```

---

## Puzzle Validation

### CompilerUI Code Normalization

```gdscript
func _normalize_code(code: String) -> String:
    var normalized = code
    normalized = normalized.replace(" ", "")   # Remove spaces
    normalized = normalized.replace("\t", "")  # Remove tabs
    normalized = normalized.replace("\n", "")  # Remove newlines
    return normalized
```

Solutions are compared after normalization, allowing flexible whitespace input.

### WordPuzzle Matching

```gdscript
const PUZZLE_TERMS = {
    "Variable": "A named storage location for data.",
    "int": "A data type for whole numbers.",
    # etc.
}

func _on_check_button_pressed():
    var all_correct = true
    for term_button in matches:
        var definition_button = matches[term_button]
        if PUZZLE_TERMS[term_button.text] != definition_button.text:
            all_correct = false
            break
    
    if all_correct:
        _on_puzzle_solved()
```

---

## Music System

**Script**: `Scenes/Music/music_manager.gd`  
**Autoload**: `MusicManager`

### Fade In

```gdscript
func fade_in_music(track: AudioStream, fade_time: float, target_volume_db: float):
    if audio_player.playing:
        await fade_out_music(fade_time)
    
    audio_player.stream = track
    audio_player.volume_db = -80  # Start silent
    audio_player.play()
    
    var tween = create_tween()
    tween.set_trans(Tween.TRANS_LINEAR)
    tween.tween_property(audio_player, "volume_db", target_volume_db, fade_time)
```

### Fade Out

```gdscript
func fade_out_music(fade_time: float):
    if not audio_player.playing:
        return
    
    var tween = create_tween()
    tween.set_trans(Tween.TRANS_LINEAR)
    tween.tween_property(audio_player, "volume_db", -80, fade_time)
    
    await tween.finished
    audio_player.stop()
```

---

## Camera System

Player has embedded Camera2D with helper functions:

```gdscript
func move_camera_to(target_position: Vector2, duration: float = 1.0):
    var camera = $Camera2D
    var tween = create_tween()
    tween.set_trans(Tween.TRANS_SINE)
    tween.set_ease(Tween.EASE_IN_OUT)
    
    var target_offset = target_position - global_position
    tween.tween_property(camera, "position", target_offset, duration)
    await tween.finished

func reset_camera(duration: float = 1.0):
    var camera = $Camera2D
    var tween = create_tween()
    tween.set_trans(Tween.TRANS_SINE)
    tween.set_ease(Tween.EASE_IN_OUT)
    tween.tween_property(camera, "position", Vector2.ZERO, duration)
    await tween.finished
```

Used for dramatic pans to NPCs or objects during cutscenes.
