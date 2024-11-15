# Homework2
# Functional and Non-Functional Requirements

## Functional Requirements (FR)
- **FR1**: The game must allow the player to toggle sound settings, enabling or muting background music and sound effects.
  - **Priority**: 1

- **FR2**: The game must allow the player to pause gameplay and provide options to resume or quit the game.
  - **Priority**: 2

## Non-Functional Requirements (NFR)
- **NFR1**: The game interface should make sound settings accessible and easy to use for players of all ages.
  - **Converted FR**: The game must display an easily accessible "Sound Settings" button on the main menu, clearly labeled for easy access.
  
- **NFR2**: The game must provide clear and distinct audio feedback when sound settings are toggled and when the game is paused or resumed.
  - **Converted FR**: The system shall produce unique sound cues for muting, unmuting, pausing, and resuming to indicate changes in audio and game state.

## Constraints
- **C1**: The game must run efficiently on both mobile and desktop platforms, supporting different screen sizes and device capabilities.
- **C2**: The game should not require any personal data input or storage to ensure player privacy.

---

## Traceability Matrix

| Non-Functional Requirement | Converted Functional Requirement(s)   |
|----------------------------|---------------------------------------|
| NFR1 (Accessible Sound Settings)     | FR1 (Sound Settings button on main menu)      |
| NFR2 (Distinct Audio Feedback)       | FR2 (Unique sound cues for audio and game states) |


## Use Case Model

### Abstract Use Cases
- **UC1**: Player toggles the sound settings
TUCBW: The player presses the button to mute or unmute the game's sound.
TUCEW: The game adjusts the audio settings, either muting or enabling sound effects and background music.

- **UC2**: Player pauses the game
TUCBW: The player presses the “pause” button during gameplay.
TUCEW: The game halts all actions and displays the paused state, giving the player the option to resume or quit the game.

# Domain Model Diagram

The following diagram represents the domain model for the game, illustrating the relationships and attributes of each class involved in the system.

![image](https://github.com/user-attachments/assets/179afe4c-4def-4eb7-85df-cefedbbd7fcb)


# Non-Trivial Steps for Use Cases
## UC1: Player toggles the sound settings

- **Step 1**: *The player presses the button to mute or unmute the game's sound.*
  - **Background Processes**:
    - **Object Modification**: The system modifies the `Settings` object associated with the `Player` to update the sound setting.
    - **Object Association**: The `Player` object maintains an association with `Settings`, which includes the sound status (`sound: Boolean`). The change in the `sound` attribute triggers an update in the system’s audio state.
  
- **Step 2**: *The game adjusts the audio settings, either muting or enabling sound effects and background music.*
  - **Background Processes**:
    - **Object Modification**: The system modifies the current audio state, muting or unmuting sound based on the `sound` attribute in the `Settings` object.
    - **Object Association**: The system references the `Settings` object tied to the current `Player` to apply the change globally across all audio components.

## UC2: Player pauses the game

- **Step 1**: *Player clicks the "pause" button during gameplay.*
  - **Background Processes**:
    - **Object Modification**: The `Game` object updates its `gameStatus` attribute to reflect the paused state.
    - **Object Creation**: A `PauseMenu` object might be created to display the pause menu with options for resuming or quitting.

- **Step 3**: *System displays the pause menu with options: "Resume" and "Quit".*
  - **Background Processes**:
    - **Object Association**: The `PauseMenu` object is associated with the `Game` object, giving the player options to interact with.
    - **Object Modification**: The system stops or freezes other game actions by modifying the `gameStatus` within the `Game` object to ensure gameplay is halted.

- **Step 5**: *If the player selects "Resume", the game continues where it left off.*
  - **Background Processes**:
    - **Object Modification**: The `Game` object’s `gameStatus` is modified to "active," resuming all game actions and unfreezing the game timer.
    - **Object Deletion**: The `PauseMenu` object may be removed or hidden from the display.

- **Step 7**: *If the player selects "Quit", the game ends and returns to the main menu.*
  - **Background Processes**:
    - **Object Modification**: The `Game` object updates its `gameStatus` to "ended" and resets its properties.
    - **Object Deletion**: The `PauseMenu` object is removed, and other game-related objects may be reset or disassociated as the player is returned to the main menu.
    - **Object Association**: The system navigates back to the main menu screen, re-associating the player with the main menu options rather than the active game.

# Design Sequence Diagrams and GRASP Justifications

# Use Case Realization for UC2: Pausing the Game

## Sequence Diagram for UC2 Step 2a: Pausing the Game

The following sequence diagram represents the interaction flow for the **UC2: Pausing the Game** use case. This diagram illustrates the actions taken when the player pauses the game, the options displayed, and the resumption of gameplay.

![image](https://github.com/user-attachments/assets/b061d53a-5830-4661-b7bb-9226e094e0cf)


---

## Participants

- **Player**: Represents the user initiating the pause action.
- **GameController**: Acts as the central controller handling the player’s actions, such as pausing and resuming the game.
- **Game**: Manages the main game state, including handling the paused status and resuming gameplay.
- **PauseMenu**: Interface displayed when the game is paused, presenting options such as "Resume" and "Quit."
- **SoundManager**: Manages audio settings, muting and unmuting sounds during pause and resume actions.

---

## Interactions

1. **Player Initiates Pause**:
   - The player presses the pause button, sending a `pressPauseButton()` message to `GameController`.
   - `GameController` forwards the pause request by calling `pauseGame()` on `Game`.
   - `Game` triggers the `PauseMenu` to display with `display()` and mutes the sound by calling `muteSound()` on `SoundManager`.

2. **Player Selects Resume**:
   - The player selects the "Resume" option on the `PauseMenu` by calling `selectResume()`.
   - `PauseMenu` then calls `resumeGame()` on `Game`, unmuting sound with `unmuteSound()` on `SoundManager` and refreshing the display with `updateDisplay()` on `GameController`.

3. **Game Over Condition**:
   - If the game reaches a game-over condition, `Game` sends a `showResults()` message to `GameController`.
   - `GameController` hides the `PauseMenu` by calling `hidePauseMenu()` to conclude the game.

---

## GRASP Patterns and Justifications

- **Controller**: The **GameController** is the main controller that manages player inputs, such as the pause button press. It serves as the central point for handling interactions and delegating tasks.
  
- **Information Expert**: The **Game** class is the information expert responsible for managing the game state. It handles pausing, resuming, and checking for the game-over condition as it has access to the necessary information.

- **Low Coupling**: **Player** interacts only with **GameController** and does not directly interact with other objects. This keeps Player loosely coupled to the game logic, allowing for greater flexibility.

- **High Cohesion**: Each object has a clear responsibility:
  - `Game` manages the game state.
  - `SoundManager` is dedicated to handling sound settings.
  - `PauseMenu` provides an interface for pause options.

---

This sequence diagram and GRASP pattern explanation demonstrate the interactions required to implement the pausing functionality in the game. Each interaction follows a structured flow to maintain cohesion, low coupling, and efficient responsibility assignment.

# Design Class Diagram for UC2: Pausing the Game

![image](https://github.com/user-attachments/assets/0347c347-6e03-4801-acf1-be8989922dea)


## GRASP Patterns and Justification

### Controller
- **GameController** acts as the **Controller** for managing the pausing and resuming actions. It handles the main game state interactions and coordinates between `Player`, `Game`, and `PauseMenu`.

### Information Expert
- The **Game** class is the **Information Expert** that holds the game state (e.g., "paused", "playing") and can manage game logic accordingly. It interacts with **SoundManager** to mute and unmute sound based on the state.

### Low Coupling
- **Player** interacts only with **GameController** to initiate the pause action, without direct connections to **Game** or **PauseMenu**. This maintains low coupling between **Player** and other components of the system.

### High Cohesion
- Each class is responsible for specific, cohesive tasks:
  - **GameController** manages interactions and state control.
  - **Game** handles game states, sound, and gameplay logic.
  - **PauseMenu** displays options during pause.
  - **SoundManager** handles sound settings.
  
This structure ensures that each class has high cohesion, focusing on specific responsibilities while minimizing interdependencies.

# Use Case: UC2 - Pausing the Game

## Objective
To verify that the game can successfully enter a paused state when the player presses the pause button, display the pause menu with options to resume or quit, mute the sound while paused, and resume gameplay or exit based on player choice.

---

## Inputs and Expected Outputs

### 1. Player presses the **pause button** during gameplay.
- **Expected Output**: 
  - The game should enter a paused state.
  - The **PauseMenu** should be displayed, showing options like "Resume" and "Quit."
  - Sound should be muted to indicate the paused status.

### 2. Player selects the **"Resume"** option in the PauseMenu.
- **Expected Output**:
  - The game should exit the paused state and return to the active gameplay screen.
  - The sound should unmute, restoring background music or sound effects.
  - The display should update to show the current game state, resuming gameplay as it was before the pause.

### 3. Player selects the **"Quit"** option in the PauseMenu.
- **Expected Output**:
  - The game should end and return to the main menu screen.
  - Any game state data related to the current session should be reset or saved if applicable.
  - The game should remain muted (or sound settings should reset as defined for the main menu).

---

## Additional Test Scenarios

### Scenario 1: Pausing the Game in Different States
- **Input**: Player presses the pause button while different game elements are active (e.g., during animations, player actions).
- **Expected Output**: 
  - The game should pause all actions seamlessly, stopping animations, and freezing gameplay immediately.
  - The PauseMenu should still appear, and sound should mute regardless of game elements.

### Scenario 2: Sound Settings During Pause
- **Input**: Check if sound remains muted while the game is paused.
- **Expected Output**: 
  - Sound should stay muted as long as the game is in a paused state, regardless of other player interactions with the pause menu.
  
### Scenario 3: Resume Functionality After Pausing
- **Input**: Player resumes the game after performing certain actions in the pause menu.
- **Expected Output**:
  - The game should continue exactly from where it was paused, restoring gameplay elements (e.g., animations, player status).
  - Sound should unmute, returning to the same state as before the pause.

---

## Summary of Expected Behavior

The **UC2: Pausing the Game** use case should allow a seamless transition between active gameplay and paused states. The game should appropriately respond to the pause button, display the PauseMenu with options to resume or quit, mute and unmute sound as required, and restore the game state accurately upon resuming. Testing these inputs and outputs ensures that the pause functionality works as intended in various gameplay scenarios.
