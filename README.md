# Shadow Infiltrator

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Game Mechanics](#game-mechanics)
4. [Technical Details](#technical-details)
5. [Installation](#installation)
6. [How to Play](#how-to-play)
7. [Development](#development)
8. [Future Enhancements](#future-enhancements)
9. [Contributing](#contributing)
10. [License](#license)
11. [Acknowledgements](#acknowledgements)

## Introduction

Shadow Infiltrator is a 2D stealth game developed in Rust using the SDL2 library. In this game, you play as a skilled infiltrator tasked with completing various missions while avoiding detection. Use your stealth abilities, gadgets, and hacking skills to overcome challenges and complete objectives in a procedurally generated environment.

## Features

- Procedurally generated levels with rooms, corridors, and doors
- Stealth-based gameplay with visibility and noise mechanics
- Advanced enemy AI with patrolling and investigation behaviors
- Gadget system including EMPs, smoke grenades, and a hacking device
- Sound propagation system that affects enemy awareness
- Mission-based objectives with increasing difficulty
- Sprite-based graphics with basic animations
- Simple particle system for visual effects

## Game Mechanics

### Stealth System
- **Visibility**: Your visibility is affected by lighting. Stay in shadows to reduce visibility.
- **Noise**: Moving generates noise. Sneak to reduce noise at the cost of speed.
- **Sound Propagation**: Sounds travel through the environment, potentially alerting enemies.

### Gadgets
1. **EMP**: Temporarily disables electronic devices in a small radius.
2. **Smoke Grenade**: Creates a smoke cloud that obscures vision, allowing you to pass undetected.
3. **Hacking Device**: Allows you to hack terminals to complete objectives or disable security systems.

### Enemy AI
- Enemies patrol designated routes and respond to suspicious activities.
- If an enemy detects a sound, they will investigate the source.
- Enemies have a cone of vision and will chase the player if spotted.

### Mission System
- Each level has specific objectives that must be completed.
- Objectives may include reaching locations, hacking terminals, or collecting intelligence.
- Complete all objectives to advance to the next level.

## User Interface and Scoring

Shadow Infiltrator features a comprehensive UI that provides players with vital information:

- **Score**: Your current score, updated in real-time based on your performance.
- **Time**: The elapsed time for the current level.
- **Objectives**: A count of completed objectives out of the total for the level.
- **Stealth Meter**: A visual representation of your stealth bonus. Avoid detection to keep this high!
- **Mini-map**: A small map in the corner showing your position and the layout of the level.
- **Messages**: Important information and feedback appear at the bottom of the screen.

### Scoring System

Your score in Shadow Infiltrator is calculated based on several factors:

1. **Objectives Completed**: Each completed objective awards 500 points.
2. **Stealth Bonus**: You start with a 1000 point stealth bonus. This decreases when alarms are triggered or you're detected.
3. **Time Bonus**: Complete the level quickly for up to 3000 bonus points.
4. **Hacking Bonus**: Each successful hack awards 100 points.

Strive for a perfect stealth run, complete all objectives, hack strategically, and finish quickly to achieve the highest possible score!




## Interactive Elements

Shadow Infiltrator features a variety of interactive elements that add depth and challenge to the gameplay:

1. **Security Cameras**: Avoid their cone of vision or hack nearby terminals to disable them.
2. **Laser Tripwires**: Carefully navigate around these or find ways to deactivate them.
3. **Computer Terminals**: Hack these to disable nearby security systems or gather intel.
4. **Ventilation Shafts**: Use these to move unseen through the facility.
5. **Power Generators**: Sabotage these to temporarily disable all security systems in an area.
6. **Locked Doors**: Find keycards to access restricted areas.

## Technical Details

Shadow Infiltrator is built using the following technologies and libraries:

- **Rust**: The main programming language used for development.
- **SDL2**: Used for rendering graphics, handling input, and managing window creation.
- **rand**: Used for random number generation in procedural level generation.

The game architecture includes the following main components:

- Game State Management
- Player and Enemy Logic
- Procedural Map Generation
- Sound System
- Rendering System
- Input Handling
- Mission and Objective Tracking

## Installation

1. Ensure you have Rust and Cargo installed on your system. If not, install them from [https://www.rust-lang.org/](https://www.rust-lang.org/)

2. Install SDL2 development libraries:
   - On Ubuntu/Debian: `sudo apt-get install libsdl2-dev libsdl2-ttf-dev libsdl2-image-dev`
   - On macOS with Homebrew: `brew install sdl2 sdl2_ttf sdl2_image`
   - On Windows, follow the SDL2 installation guide for Rust: [https://github.com/Rust-SDL2/rust-sdl2#windows](https://github.com/Rust-SDL2/rust-sdl2#windows)

3. Clone the repository:
   ```
   git clone https://github.com/taylorjohn/shadow-infiltrator.git
   cd shadow-infiltrator
   ```

4. Build and run the game:
   ```
   cargo run --release
   ```

## How to Play

- Use **WASD** keys to move your character.
- Hold **Left Shift** to sneak (move slowly but quietly).
- Press **E** to interact with objects (hack terminals, sabotage generators, enter/exit vents, open doors).
- Use **1**, **2**, and **3** keys to activate gadgets (EMP, Smoke Grenade, and Hacking Device respectively).
- Keep an eye on your Stealth Meter and try to maintain a high stealth bonus.
- Complete objectives quickly to earn a higher time bonus.
- Use the mini-map to navigate through the level efficiently.
- Pay attention to on-screen messages for important information and feedback.

Remember, a true infiltrator values stealth over confrontation. Plan your moves carefully, avoid detection, and complete your objectives with precision to achieve the highest score!

## Development

The project structure is as follows:

- `src/main.rs`: Entry point and game loop
- `src/game_state.rs`: Main game state and logic
- `src/player.rs`: Player-related functionality
- `src/enemy.rs`: Enemy AI and behavior
- `src/map.rs`: Map generation and management
- `src/sound.rs`: Sound system implementation
- `src/mission.rs`: Mission and objective system
- `src/render.rs`: Rendering functions
- `src/input.rs`: Input handling
- `src/sprite.rs`: Sprite and spritesheet management

## Future Enhancements

- Implement more sophisticated enemy AI, including different enemy types.
- Add more interactive elements to the map, such as security cameras or traps.
- Expand the gadget system with more diverse and interesting tools.
- Implement a more complex sound propagation system that considers obstacles.
- Add a particle system for enhanced visual effects.
- Implement a save/load system for game progress.
- Add background music and more sound effects.
- Create a level editor for custom level design.

## Contributing

Contributions to Shadow Infiltrator are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch for your feature (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Please ensure your code adheres to the existing style and passes all tests.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgements

- SDL2 Rust bindings: [https://github.com/Rust-SDL2/rust-sdl2](https://github.com/Rust-SDL2/rust-sdl2)
- Rust programming language: [https://www.rust-lang.org/](https://www.rust-lang.org/)

This game draws inspiration from several classic and modern stealth games:
- Metal Gear series, particularly the early 2D installments
- Thief series, for its innovative use of light and shadow in stealth mechanics
- Mark of the Ninja, demonstrating effective 2D stealth gameplay
- Hotline Miami, for its top-down perspective and tight gameplay in enclosed spaces
- Commandos series, showcasing top-down tactical stealth gameplay

We are grateful to the developers of these games for pushing the boundaries of the stealth genre and inspiring new generations of game developers.

