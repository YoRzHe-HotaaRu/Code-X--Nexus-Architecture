# NPC Guide

Documentation of all NPCs in Code[X]:Nexus.

---

## NPC Types

| Type | Purpose | Puzzle Type |
|------|---------|-------------|
| **Story** | Advance narrative, provide lore | None |
| **Teacher** | Teach C++ concepts | CompilerUI / WordPuzzle / Quiz |
| **Guardian** | Block progress until condition met | Dialogue |
| **Boss** | Final antagonist | Multi-phase battle |

---

## World Start NPCs

### Aliff
- **Role**: Story introduction
- **Location**: Near campfire
- **Dialogue**: Welcomes player, explains corruption

### Jack (Jackal)
- **Role**: Backstory provider
- **Location**: Village area
- **Dialogue**: Lore about the Nexus

### Adli
- **Role**: Tutorial NPC
- **Type**: Quiz dialogue
- **Teaches**: How to interact and make choices

### Maya
- **Role**: Key item giver
- **Item**: The Nexus (chatbot access)
- **Unlocks**: ChatbotUI button in HUD

### Alya
- **Role**: Quiz NPC
- **Type**: Dialogue-based quiz
- **Teaches**: Basic C++ concept recognition

---

## World 1 NPCs

### Zafran
- **Role**: First puzzle NPC
- **Type**: CompilerUI
- **Quest**: `solved_darkness_puzzle`
- **Topic**: Variable types
- **Effect**: Solving removes world darkness

### Bukh
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_bukh_puzzle`
- **Topic**: Data types

### Lyra
- **Role**: Teacher
- **Type**: WordPuzzle
- **Quest**: `solved_lyra_puzzle`
- **Topic**: C++ term definitions
- **Special**: Provides location hints after success

### Shafiq
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_shafiq_puzzle`
- **Topic**: Syntax correction

### Asad
- **Role**: Teacher
- **Type**: Dialogue Quiz
- **Quest**: `completed_asad_quiz`
- **Topic**: Code comprehension
- **Tracks**: `asad_quiz_attempts` for retry count

---

## World 2 NPCs

### Niko
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_niko_puzzle`
- **Topic**: Functions
- **Effect**: Left door light turns green

### Aliff R
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_aliffr_puzzle`
- **Topic**: Advanced syntax
- **Effect**: Right door light turns green

### Adriana
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_adriana_puzzle`
- **Topic**: Operators

### Zul
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_zul_puzzle`
- **Topic**: Function parameters

### Aqil
- **Role**: Teacher
- **Type**: CompilerUI
- **Quest**: `solved_aqil_puzzle`
- **Topic**: Return values

---

## World 3 NPCs

### Guardian NPCs (Block Path)

These 4 NPCs form a barrier. Each teaches a concept through dialogue:

| NPC | Topic | Quest ID |
|-----|-------|----------|
| **Erin** | Conditionals | `learned_conditionals` |
| **Akari** | Loops | `learned_loops` |
| **Isyraq** | Variables | `learned_variables` |
| **Rapxstor** | Functions | `learned_functions` |

When all 4 are complete:
- NPCs fade out and disappear
- Barrier is removed
- Camera pans to Corrupter area
- "Final Act: Corrupter's Desire" title shows

---

## The Corrupter (Boss)

**NPC**: `corrupter_yazid_npc.gd`  
**Real Name**: Yazid  
**Role**: Main antagonist

### Dialogue Themes
- Arrogance about rewriting the system
- Mocking player's debugging attempts
- Frustration as player succeeds

### Battle Phases

**Phase 1: Compiler Puzzles (3 rounds)**

| Round | Bug Type | Topic |
|-------|----------|-------|
| 1 | Type mismatch | Variables |
| 2 | Condition logic | Conditionals |
| 3 | Infinite loop | Loops |

**Phase 2: Word Puzzles (2 rounds)**

| Puzzle | Content |
|--------|---------|
| WordPuzzle5 | C++ concepts |
| WordPuzzle6 | Code terminology |

**Phase 3: Final Challenge**

Complex bug combining multiple concepts:
- Loop iterates 100 times
- Contains conditional inside
- Logic is inverted (skips corrupted instead of fixing)
- **Solution**: Change logic to call `RestoreSector(i)` when corrupted

### Defeat Sequence
1. Victory dialogue plays
2. Corrupter fades/distorts
3. Exit portal appears
4. Player monologue about returning home
5. Portal leads to credits

---

## NPC Script Pattern

All NPCs follow this structure:

```gdscript
extends StaticBody2D

# Dialogue arrays
@export var dialogue_data_intro: Array
@export var dialogue_data_success: Array
@export var dialogue_branches: Dictionary

var player_in_range = null

func _ready():
    SignalManager.quest_step_completed.connect(_on_quest_step_completed)

func interact():
    QuestManager.complete_objective("met_npcname")
    
    if QuestManager.quest_data["solved_npc_puzzle"]:
        SignalManager.show_dialogue.emit(dialogue_data_success, {})
    else:
        SignalManager.show_dialogue.emit(dialogue_data_intro, dialogue_branches)

func _on_quest_step_completed(quest_id: String):
    if quest_id == "start_npc_puzzle":
        CompilerUI.open_compiler(puzzle_data, ...)
```

---

## Character Portraits

All characters have portraits stored in:
```
Resources/Characters/[CharacterName]/[Portrait].png
```

Some NPCs have multiple expressions:
- Default (talking)
- Success (happy)
- Fail (disappointed)
