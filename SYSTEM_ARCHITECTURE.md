# System Architecture

Technical documentation for Code[X]:Nexus architecture and systems.

---

## Autoload Singletons

The game uses **21 autoloaded singletons** for global state and UI management:

| Autoload | Script/Scene | Purpose |
|----------|--------------|---------|
| `DialogueUI` | dialogue_ui.tscn | Branching dialogue system |
| `SignalManager` | signal_manager.gd | Global event bus |
| `GameHUD` | game_hud.tscn | HUD, chapter titles, fades |
| `ChatbotUI` | chatbot_ui.tscn | AI-powered C++ assistant |
| `CompilerUI` | compiler_ui.tscn | Code-fixing puzzle interface |
| `QuestManager` | quest_manager.gd | Quest/objective tracking |
| `QuestUI` | quest_ui.tscn | Quest display panel |
| `PauseMenu` | pause_menu.tscn | Pause menu |
| `ItemGetPopup` | item_get_popup.tscn | Item acquisition popup |
| `MusicManager` | music_manager.tscn | Audio fade in/out |
| `MapScene` | map_scene.tscn | World map with notes |
| `SceneLoader` | SceneLoader.gd | Scene transitions |
| `PuzzleManager` | puzzle_manager.gd | Puzzle portrait storage |
| `WordPuzzle` 1-6 | WordPuzzle.tscn | Term-matching puzzles |
| `FinalChallengeUI` | final_challenge_ui.tscn | Boss battle initiation |
| `OptionsMenu` | options_menu.tscn | Settings menu |

---

## Signal-Based Event System

`SignalManager` serves as a **central event bus** for loose coupling between systems:

```mermaid
flowchart LR
    subgraph Emitters
        NPC[NPCs]
        World[Worlds]
        UI[UI Systems]
    end
    
    subgraph SignalManager
        DS[dialogue_started]
        DE[dialogue_ended]
        SD[show_dialogue]
        QS[quest_step_completed]
        CO[chatbot_opened/closed]
        MO[map_opened/closed]
        EC[environment_color_changed]
    end
    
    subgraph Listeners
        Player[Player]
        QM[QuestManager]
        GUI[GameHUD]
    end
    
    NPC --> SD
    NPC --> QS
    World --> EC
    UI --> CO
    UI --> MO
    
    DS --> Player
    DE --> Player
    QS --> QM
    EC --> World
```

### Key Signals

| Signal | Emitted By | Purpose |
|--------|------------|---------|
| `dialogue_started` | DialogueUI | Freezes player movement |
| `dialogue_ended` | DialogueUI | Unfreezes player |
| `show_dialogue` | NPCs/Worlds | Triggers dialogue display |
| `quest_step_completed` | DialogueUI/Puzzles | Updates quest progress |
| `environment_color_changed` | NPCs | Triggers world lighting changes |

---

## Scene Hierarchy

```mermaid
graph TD
    Root[project.godot] --> MainMenu[MainMenu/]
    Root --> Scenes[Scenes/]
    Root --> Resources[Resources/]
    
    Scenes --> World[World/]
    Scenes --> NPC[NPC/]
    Scenes --> GUI[GUI/]
    Scenes --> Quest[Quest/]
    Scenes --> Dialogue[Dialogue/]
    Scenes --> Player[Player/]
    Scenes --> Cutscenes[Cutscenes/]
    Scenes --> Music[Music/]
    
    World --> WS[world_start.tscn]
    World --> W1[world_1.tscn]
    World --> W2[world_2.tscn]
    World --> W3[world_3.tscn]
    
    Quest --> QM[quest_manager.gd]
    Quest --> WP[WordPuzzle 1-6]
    Quest --> FC[final_challenge_ui]
```

---

## Data Flow

### Player Interaction Loop

```mermaid
sequenceDiagram
    participant P as Player
    participant NPC as NPC
    participant SM as SignalManager
    participant DUI as DialogueUI
    participant QM as QuestManager
    
    P->>NPC: E key (interact)
    NPC->>SM: show_dialogue.emit()
    SM->>DUI: start_dialogue()
    DUI->>SM: dialogue_started.emit()
    SM->>P: can_move = false
    
    loop Dialogue Lines
        P->>DUI: E key (advance)
        DUI->>DUI: show_next_line()
    end
    
    DUI->>SM: dialogue_ended.emit()
    SM->>P: can_move = true
    DUI->>SM: quest_step_completed.emit()
    SM->>QM: complete_objective()
```

---

## Physics Layers

| Layer | Name | Used By |
|-------|------|---------|
| 1 | Environment | Walls, obstacles |
| 2 | Player | Player character |
| 3 | Puzzle Elements | Interactive objects |
| 4 | Puzzle Walls | Blocking barriers |
| 5 | Enemies | Boss, hazards |

---

## Input Mappings

| Action | Keys | Purpose |
|--------|------|---------|
| `move_right` | D | Movement |
| `move_up` | W | Movement |
| `move_left` | A | Movement |
| `move_down` | S | Movement |
| `interact` | E | NPC/object interaction |
| `toggle_quest_ui` | TAB | Show/hide quest panel |
| `ui_pause` | P, ESC | Pause menu |

---

## Rendering Configuration

- **Viewport**: 960x540 pixels
- **Stretch Mode**: canvas_items
- **Renderer**: GL Compatibility
- **Texture Filter**: Nearest (pixel-perfect)
- **2D Transform Snap**: Enabled
