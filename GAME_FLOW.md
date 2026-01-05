# Game Flow

Documentation of gameplay progression and player journey through Code[X]:Nexus.

---

## High-Level Flow

```mermaid
flowchart TD
    MM[Main Menu] --> TC[Text Cutscene]
    TC --> WS[World Start - Prologue]
    WS --> W1[World 1 - Act I]
    W1 --> W2[World 2 - Act II]
    W2 --> W3[World 3 - Act III]
    W3 --> Boss[Final Boss Battle]
    Boss --> Credits[Credits/End]
```

---

## Main Menu Flow

```mermaid
flowchart LR
    Start[Game Launch] --> FadeIn[Fade In Animation]
    FadeIn --> Menu{Main Menu}
    Menu -->|Play| Reset[Reset Quest Progress]
    Reset --> FadeOut[Fade Out]
    FadeOut --> Cutscene[Text Cutscene]
    Menu -->|Options| Options[Options Menu]
    Menu -->|Quit| Exit[Exit Game]
```

**Key Actions:**
- `QuestManager.reset_all_quests()` is called on "Play" to ensure fresh start
- Music fades in on menu load, fades out before scene transition

---

## Act Structure

| Act | World Scene | Chapter Title | Theme |
|-----|-------------|---------------|-------|
| **Prologue** | world_start | "Why am I here?" | Awakening, introduction |
| **Act I** | world_1 | "A Glimpse of Light" | Garden/mansion, darkness puzzle |
| **Act II** | world_2 | "The One Behind it All" | Laboratory, tech puzzles |
| **Act III** | world_3 | "The Horizon's End" | Glitched void, final confrontation |
| **Final Act** | world_3 (cont.) | "Corrupter's Desire" | Boss battle |

---

## Per-World Flow

### World Entry Sequence

```mermaid
sequenceDiagram
    participant W as World Scene
    participant GH as GameHUD
    participant MM as MusicManager
    participant SM as SignalManager
    participant DUI as DialogueUI
    
    W->>GH: play_fade_in(1.0)
    W->>MM: fade_in_music()
    
    alt First Visit
        W->>SM: dialogue_started.emit()
        W->>DUI: show_dialogue(intro_monologue)
        DUI->>SM: dialogue_ended
        W->>GH: show_chapter_title()
        W->>SM: dialogue_ended.emit()
    end
    
    W->>W: QuestUI.show_quest_context()
```

---

## Player Gameplay Loop

```mermaid
flowchart TD
    Explore[Explore World] --> Find[Find NPC]
    Find --> Talk[Talk to NPC]
    Talk --> Choice{Quest Type}
    
    Choice -->|Tutorial| Learn[Learn Concept via Dialogue]
    Choice -->|Puzzle| Puzzle[Open Puzzle UI]
    Choice -->|Story| Story[Advance Story]
    
    Puzzle --> Solve{Solved?}
    Solve -->|Yes| Complete[Complete Objective]
    Solve -->|No| Retry[Try Again]
    Retry --> Puzzle
    
    Learn --> Complete
    Story --> Complete
    
    Complete --> Update[Quest UI Updates]
    Update --> Check{All Objectives Done?}
    Check -->|Yes| Portal[Portal Activates]
    Check -->|No| Explore
    
    Portal --> NextWorld[Next World]
```

---

## Puzzle Interaction Flow

### CompilerUI (Code Fixing)

```mermaid
sequenceDiagram
    participant NPC
    participant CUI as CompilerUI
    participant P as Player
    participant QM as QuestManager
    
    NPC->>CUI: open_compiler(problem_data)
    CUI->>CUI: Display broken code
    P->>CUI: Type solution
    P->>CUI: Click Run
    
    alt Correct Solution
        CUI->>CUI: Show success portrait
        CUI->>CUI: Play correct SFX
        CUI->>QM: quest_step_completed.emit()
        CUI->>CUI: Auto-close after 2s
    else Incorrect
        CUI->>CUI: Show fail portrait
        CUI->>CUI: Play wrong SFX
        CUI->>CUI: Reset after 2s
    end
```

### WordPuzzle (Term Matching)

```mermaid
sequenceDiagram
    participant NPC
    participant WP as WordPuzzle
    participant P as Player
    participant QM as QuestManager
    
    NPC->>WP: open_puzzle()
    WP->>WP: Shuffle terms/definitions
    
    loop Until All Matched
        P->>WP: Click term
        P->>WP: Click definition
        WP->>WP: Highlight match
    end
    
    P->>WP: Click Check
    
    alt All Correct
        WP->>QM: complete_objective()
        WP->>WP: Close puzzle
    else Has Errors
        WP->>WP: Highlight incorrect (red)
        WP->>WP: Reset after 2s
    end
```

---

## Scene Transition Pattern

All world transitions use a consistent pattern:

1. **Fade Out**: `GameHUD.play_fade_out(1.0)`
2. **Music Fade**: `MusicManager.fade_out_music(1.0)`
3. **Scene Change**: `get_tree().change_scene_to_file()`
4. **Fade In**: New world calls `GameHUD.play_fade_in(1.0)`
5. **Music Start**: `MusicManager.fade_in_music()`

---

## Boss Battle Flow (World 3)

```mermaid
flowchart TD
    Enter[Enter Trigger Area] --> Cutscene[Intro Cutscene]
    Cutscene --> Dialog1[Corrupter Dialogue]
    Dialog1 --> PlayerReply[Player Reply]
    PlayerReply --> Challenge[Face Off Button]
    
    Challenge --> P1[Phase 1: 3 Compiler Puzzles]
    P1 --> P1D[Corrupter Reaction Dialogue]
    P1D --> P2[Phase 2: 2 Word Puzzles]
    P2 --> P2D[Corrupter Reaction Dialogue]
    P2D --> P3[Phase 3: Final Compiler Puzzle]
    P3 --> Victory[Victory Cutscene]
    Victory --> Portal[Exit Portal Appears]
    Portal --> Credits[Credits Scene]
```
