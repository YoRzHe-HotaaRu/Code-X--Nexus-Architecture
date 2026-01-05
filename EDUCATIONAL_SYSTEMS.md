# Educational Systems

Documentation of C++ learning components in Code[X]:Nexus.

---

## Overview

Code[X]:Nexus teaches **introductory C++ programming** through three integrated educational systems:

| System | Type | Topics |
|--------|------|--------|
| **CompilerUI** | Code-fixing puzzles | Variables, types, conditionals, loops, functions |
| **WordPuzzle** | Term-definition matching | C++ vocabulary and concepts |
| **ChatbotUI** | AI Q&A assistant | Open-ended C++ questions |

---

## CompilerUI (Code Fixing)

**Script**: `Scenes/GUI/compiler_ui.gd`

### Purpose
Players fix broken C++ code to progress. Teaches debugging and syntax comprehension.

### Interface

```
┌────────────────────────────────────────────┐
│ [Problem Display - Broken Code with Colors]│
├────────────────────────────────────────────┤
│ [Code Input - Player types solution]       │
├────────────────────────────────────────────┤
│ [Hint Label]                   [Run Button]│
└────────────────────────────────────────────┘
         [NPC Portrait]   [Close Button]
```

### Problem Data Structure

```gdscript
var puzzle_data = {
    "quest_id": "objective_id",
    "hint": "Helpful hint for the player",
    "solution": "int x = 5;",
    "broken_code": "[color=red]string x = 5;[/color] // BUG!"
}
```

### Validation Logic
1. Player types solution
2. Code is normalized (whitespace removed)
3. Compared against stored solution
4. Success: Quest completes, UI closes
5. Failure: Error message, retry allowed

### Example Puzzles

**World 1 - Variable Types**:
```cpp
// BUG: Type mismatch
int name = "Alan";  // Should be: string name = "Alan";
```

**Boss Phase 1 - Infinite Loop**:
```cpp
// BUG: Wrong increment
for(int i=0; i<5; i--) { ... }  // Should be: i++
```

---

## WordPuzzle (Term Matching)

**Scripts**: `Scenes/Quest/word_puzzle.gd` (plus WordPuzzle2-6 variants)

### Purpose
Players match C++ terms to definitions. Reinforces vocabulary and concept understanding.

### Interface

```
┌─────────────────┬─────────────────────────────┐
│    TERMS        │      DEFINITIONS            │
├─────────────────┼─────────────────────────────┤
│ [ Variable ]    │ [ A data type for integers ]│
│ [ int      ]    │ [ Stores text/strings      ]│
│ [ string   ]    │ [ Named storage location   ]│
└─────────────────┴─────────────────────────────┘
     [Check] [Reset]        [NPC Portrait]
```

### Term Sets

**WordPuzzle 1 (Lyra)** - Basic Concepts:
| Term | Definition |
|------|------------|
| Variable | A named storage location for data |
| int | A data type for whole numbers |
| float | A data type for decimal numbers |
| string | A sequence of characters |
| cout | Output data to console |
| cin | Get input from user |

### Mechanic
1. Click term → Highlight yellow
2. Click definition → Creates match (green)
3. All matched → Check button enables
4. Check → Validates all matches
5. Correct: Quest completes
6. Incorrect: Highlights wrong matches in red

---

## ChatbotUI (AI Assistant)

**Script**: `Scenes/GUI/chatbot_ui.gd`

### Purpose
AI-powered C++ learning companion for open-ended questions.

### Technology
- **API**: OpenRouter API
- **Model**: GPT-based model
- **Format**: Chat history with context

### System Prompt Extract

```
You are a helpful companion in Code[X]: Nexus...
- ONLY answer C++ programming questions
- If asked about game directions: "Search around for clues..."
- Keep responses short, concise and fun
- FORMAT: Simple text with code blocks
```

### Interface

```
┌─────────────────────────────────────────┐
│ [Chat History - RichTextLabel]          │
│ System: Greetings, traveler...          │
│ Player: How do pointers work?           │
│ Nexus: Pointers store memory addresses..│
├─────────────────────────────────────────┤
│ [Input Box]              [Send] [Close] │
└─────────────────────────────────────────┘
```

### Features
- Markdown to BBCode conversion
- Code block formatting
- Response length adapts to question complexity
- Context-aware conversation history

---

## C++ Topics Covered

### By World

| World | Topics | Teaching Method |
|-------|--------|-----------------|
| **Start** | Introduction | Dialogue |
| **1** | Variables, Data Types, Basic Syntax | CompilerUI + WordPuzzle |
| **2** | Functions, Operators | CompilerUI |
| **3** | Review (All topics) | WordPuzzle + Boss Battle |

### Concept Breakdown

**Variables & Data Types**:
- `int`, `float`, `string`, `bool`, `char`
- Variable declaration and initialization
- Type mismatches

**Control Flow**:
- `if`/`else` conditionals
- Comparison operators (`<`, `>`, `==`)
- `for` loops and `while` loops

**Functions**:
- Function declaration
- Parameters and return types
- Function calls

**I/O Operations**:
- `cout` for output
- `cin` for input
- Stream operators `<<` and `>>`

---

## Boss Battle Educational Design

The final boss uses all three puzzle types in a 3-phase battle:

### Phase 1: Compiler Puzzles (3 rounds)
1. **Variable Type Mismatch** - Fix `int target = "text"`
2. **Conditional Logic Error** - Fix `if (health < 0)` when health is 100
3. **Infinite Loop** - Fix `for(i=0; i<5; i--)` decrement bug

### Phase 2: Word Puzzles (2 rounds)
- WordPuzzle5: C++ concepts
- WordPuzzle6: Code terminology

### Phase 3: Final Compiler Challenge
- Complex bug: Logic inside loop is inverted
- Tests comprehension of conditionals + loops combined

This structure ensures players must demonstrate mastery of all concepts to complete the game.
