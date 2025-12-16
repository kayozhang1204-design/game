# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file interactive narrative game: "Boardroom Survival - A Leadership Game". A brandless corporate leadership simulation set at a beverage company post-acquisition. Built entirely with vanilla HTML, CSS, and JavaScript in one self-contained HTML file.

## Running the Game

Open the HTML file directly in a browser - no build step or server required.

## Architecture

### Single-File Structure
Everything is contained in one HTML file:
- `<style>` block: Retro pixel-art CSS with VT323/Press Start 2P fonts, CRT effects, CSS box-shadow pixel sprites
- `<body>`: Game screens (title, meet-players, game, ending) plus player modal
- `<script>` block: Game engine and content

### Game State
```javascript
gameState = {
    currentScene: 0,           // Current scene index
    credibility: 50,           // Score 0-100
    politicalSavvy: 50,        // Score 0-100
    allies: [],                // Array of character names
    choices: {},               // Player's choice history
    characterEmotions: {}      // Current emoji states
}
```

### Content Data Structures
- `characters`: Character metadata (name, title, icon)
- `characterProfiles`: Detailed backstories for meet-players screen
- `scenes[]`: Array of 33+ scene objects with dialogue, choices, effects, and branching
- `sceneIllustrations`: Maps scene index to background/character sprites
- `endings`: Four outcome objects (promoted, projectLead, sidelined, fired) with lessons

### Scene Object Format
```javascript
{
    speaker: "roger",          // Character key or "narrator"
    text: "dialogue...",       // Main dialogue text
    emotion: "description",    // Emotion hint shown below dialogue
    setEmotion: { roger: "😠" }, // Update character emotions
    choices: [{                // Optional branching choices
        label: "A) Option",
        text: "What you say",
        description: "hint",
        next: 7,               // Next scene index or "ENDING"
        effects: { credibility: 10, politicalSavvy: -5 },
        addAlly: "david",
        setEmotions: {}
    }],
    next: 6                    // Auto-advance (when no choices)
}
```

### Key Functions
- `showScene(index)`: Renders scene, dialogue, and choices
- `makeChoice(choice)`: Applies effects, updates state, navigates
- `showEnding()`: Calculates ending type from scores/allies, displays result
- `updateSceneIllustration()`: Updates pixel-art scene background and characters

### Ending Calculation
```javascript
if (avgScore >= 80 && allies >= 4) → 'promoted'
else if (avgScore >= 55 && allies >= 2) → 'projectLead'
else if (avgScore >= 35 || allies >= 1) → 'sidelined'
else → 'fired'
```

## Styling Notes

Pixel art characters are rendered using CSS `box-shadow` with 4x scaling. Scene backgrounds use layered CSS gradients. The retro aesthetic relies on step-based animations and dashed borders.

## Development Workflow

**Git Commit Rule**: After completing each modular change (new feature, bug fix, refactor, style update, or content addition), create a Git commit with a descriptive message. This ensures every incremental change is versioned and can be reverted if needed. Do not batch multiple unrelated changes into a single commit.
