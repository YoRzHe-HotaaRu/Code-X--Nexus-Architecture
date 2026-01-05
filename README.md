# Code[X]:Nexus

<div align="center">

**A Narrative-Driven 2D Top-Down Adventure Game for Learning Introductory C++ Programming**

*Research Area: Game-Based Learning (GBL)*

![Godot 4.5](https://img.shields.io/badge/Godot-4.5-478CBF?style=for-the-badge&logo=godot-engine&logoColor=white)
![GDScript](https://img.shields.io/badge/GDScript-355570?style=for-the-badge&logo=godot-engine&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)

</div>

---

## 📖 About

**Code[X]:Nexus** is an educational adventure game that teaches introductory C++ programming concepts through immersive storytelling and interactive puzzles. Players explore a corrupted digital world where they must "debug reality" by solving programming challenges to defeat The Corrupter and restore the Nexus.

### The Story

> *"In the beginning, there was only the 'Source Code'... A perfect, complex language that bound reality together, C++. It was woven into a grand construct known as the Nexus, the core of our world. But a syntax error, deep in the core, has caused a disruption..."*

You are an **anomaly** — an unhandled exception trapped in a failing system. Navigate through four distinct worlds, meet NPCs who teach you C++ concepts, solve code-based puzzles, and ultimately face The Corrupter in an epic debugging battle.

---

## ✨ Features

### 🎮 Gameplay
- **4 Unique Worlds** with progressive difficulty and distinct themes
- **19+ NPCs** with branching dialogue and unique personalities
- **Boss Battle** with 3-phase C++ debugging challenges
- **Quest System** tracking 65+ objectives across all worlds

### 📚 Educational Components
- **CompilerUI**: Fix broken C++ code with syntax highlighting
- **WordPuzzle**: Match C++ terms with their definitions
- **AI Chatbot**: GPT-powered C++ learning companion (Nexus)
- **Topics Covered**: Variables, Data Types, Loops, Conditionals, Functions

### 🎨 Presentation
- **2D Pixel Art** style with animated world objects
- **Dynamic Environment** effects (darkness, glitch shaders)
- **Chapter Titles** and cinematic cutscenes
- **Adaptive Music** system with fade transitions

---

## 🗂️ Project Structure

```
code[x]-nexus/
├── MainMenu/           # Main menu scene and scripts
├── Resources/          # Assets, fonts, characters, BGM
│   ├── Assets/         # Sprites, tilesets (12,600+ files)
│   ├── BGM/            # Background music tracks
│   ├── Characters/     # Character portraits and sprites
│   └── Font/           # Custom fonts
├── Scenes/             # Game scenes and logic
│   ├── Cutscenes/      # Intro text cutscene
│   ├── Dialogue/       # Dialogue UI and signal manager
│   ├── GUI/            # HUD, menus, chatbot, compiler UI
│   ├── LoadingScreen/  # Scene transitions
│   ├── Music/          # Music manager
│   ├── NPC/            # All NPC scenes and scripts
│   ├── Player/         # Player controller
│   ├── Quest/          # Quest manager and puzzle systems
│   ├── World/          # World scenes (start, 1, 2, 3)
│   └── World_Objects/  # Portals, decorations, interactive objects
└── project.godot       # Godot project configuration
```

---

## 🚀 Getting Started

### Prerequisites
- [Godot 4.5](https://godotengine.org/download) or later (GL Compatibility renderer)

### Running the Game
1. Clone or download this repository
2. Open Godot Engine
3. Click **Import** and select the `project.godot` file
4. Press **F5** or click **Run Project**

### Controls
| Key | Action |
|-----|--------|
| **W/A/S/D** | Move |
| **E** | Interact |
| **P / ESC** | Pause Menu |
| **TAB** | Toggle Quest UI |

---

## 📜 Game Progression

| Act | World | Theme | Learning Focus |
|-----|-------|-------|----------------|
| **Prologue** | world_start | Campfire Village | Introduction, Meeting NPCs |
| **Act I** | world_1 | Garden Mansion | Variables, Data Types, Syntax |
| **Act II** | world_2 | Tech Laboratory | Functions, Operators |
| **Act III** | world_3 | Horizon's End | Review + Final Boss Battle |

---

## 🛠️ Technologies

- **Game Engine**: Godot 4.5 (GL Compatibility)
- **Language**: GDScript
- **AI Integration**: OpenRouter API (GPT) for chatbot
- **Physics Layers**: Environment, Player, Puzzle Elements, Enemies

---

## 📚 Documentation

- [SYSTEM_ARCHITECTURE.md](SYSTEM_ARCHITECTURE.md) - Technical architecture
- [GAME_FLOW.md](GAME_FLOW.md) - Gameplay progression
- [WORLD_FLOW.md](WORLD_FLOW.md) - World structure and objectives
- [GAME_LOGIC.md](GAME_LOGIC.md) - Core game systems
- [EDUCATIONAL_SYSTEMS.md](EDUCATIONAL_SYSTEMS.md) - Learning components
- [NPC_GUIDE.md](NPC_GUIDE.md) - Character documentation

---

## 👨‍💻 Author

Created as a **Final Year Project (FYP)** for Bachelor's Degree

---

## 📄 License

This project is for educational purposes.

---

<div align="center">

**Debug Reality. Restore the Nexus. Learn C++.**

</div>
