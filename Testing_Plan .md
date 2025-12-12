Testing Plan — 

1\. Overview  
Describe in several sentences what this document is and what it will cover

Description of the Testing Plan for the game “Find the Key,” a game where the player moves around the map searching for a key to open the door and win, but loses if they encounter an enemy. This document details the User Stories and their acceptance criteria, the test cases, the limitations, and the opportunities for game automation.

\---

2\. User Stories and Acceptance Criteria

User Story 1: As a player, I want to move around the map to explore and find the key  
\- Acceptance Criterion 1: The player can enter a valid move command    
    \- Requirement(s): The game accepts the “Move” command  
    \- Purpose: Verify that the movement works  
    \- Test Case:  
        a) Feature: Basic movement characteristics  
        b) Test: Enter “Move”  
        c) Expected Result / Behavior:The player's position changes  
        d) Automation Candidate: Yes

\- Acceptance Criterion 2: Invalid entries do not break the game   
    \- Requirement(s): Error handling  
    \- Purpose: Avoid errors due to unrecognized commands  
    \- Test Case:  
        a) Feature: Input Validation  
        b) Test: The player types “m0ve”  
        c) Expected Result / Behavior: It's not moving, the movement command is invalid.  
        d) Automation Candidate: Yes

User Story 2: As a player, I want the game to generate events after I move  
\- Acceptance Criterion 1: The game generates a random event after each move  
    \- Requirement(s): random.choice(Enemy, Nothing, Key)  
    \- Purpose: Validate consistency  
    \- Test Case:  
        a) Feature: Event system  
        b) Test: Make multiple movements  
        c) Expected Result / Behavior:Each movement generates an event  
        d) Automation Candidate: Yes (high-volume)

\- Acceptance Criterion 2: If the player encounters an enemy, they lose.  
    \- Requirement(s): The game ends, “You lost”  
    \- Purpose: Test completion logic  
    \- Test Case:   
        a) Feature: Encounter with enemy  
        b) Test: Force “enemy” event  
        c) Expected Result / Behavior: Player death \- game over  
        d) Automation Candidate: Yes

User Story 3:  As a player, I want to win when I open the door after finding the key.  
\- Acceptance Criterion 1: The game detects when the player reaches the key  
    \- Requirement(s): Collision detection  
    \- Purpose: Key recognition in inventory  
    \- Test Case:  
        a) Feature: Detection of the “key” object  
        b) Test: Move to the key cell  
        c) Expected Result / Behavior: The key appears in the player's inventory.  
        d) Automation Candidate: Yes

\- Acceptance Criterion 2: The game ends correctly after opening the door with the key  
    \- Requirement(s): game\_active \= False  
    \- Purpose: Verify game closure is correct  
    \- Test Case:   
        a) Feature: Game over  
        b) Test: Open door with the key  
        c) Expected Result / Behavior: The game stops  
        d) Automation Candidate: No

\---

3\. Boundary and Edge Testing

Boundary / Edge 1:  Player movement   
    \- Requirement(s): Do not leave the map, do not go through walls, do not execute movement with invalid commands   
    \- Purpose: Avoid invalid positions, avoid execution of invalid commands 

    \- Test Case 1:   
        a) Upper Boundary: Player at (0,0) tries to move left. Wall at (2,1), player at (1,1), player tries to move right  
        b) Testing data used: \< x \- 1 , x, x \+ 1 \>  
        c) Expected Result / Behavior: The player does not move, the position does not change  
        d) Automation Candidate: Yes

    \- Test Case 2:   
        a) Lower Boundary: The player enters invalid commands, or enters too many commands at once.  
        b) Testing data used:  "m0ve" → invalid, "move up up" → invalid, 20+ commands in 1 second → invalid          
        c) Expected Result / Behavior: Do not execute movement with invalid commands; process a specified limit of commands.  
        d) Automation Candidate: Yes

Boundary / Edge 2:  Key Status  
    \- Requirement(s): The player cannot collect more than one key; to win, the door must be opened with the key, and the key must be in a valid state.   
    \- Purpose: Avoid collecting more than one key; check the condition of the key.

    \- Test Case 1:   
        a) Upper Boundary: The player returns to the cell and tries to retrieve the key again.  
        b) Testing data used: When player.has\_key \= True, the key is retrieved from the cell again.  
        c) Expected Result / Behavior: player.has\_key remains “True”, the counter does not increment,"You already have the key"  
        d) Automation Candidate: Yes

    \- Test Case 2:   
        a) Lower Boundary: The player attempts to open the door without a key; the key status is invalid.  
        b) Testing data used: Player on player.has\_key \= False, tries to open the door. Player.has\_key \= “yes” or null  
        c) Expected Result / Behavior: Do not end game“You need the key”, player.has\_key debe ser True  
        d) Automation Candidate: Yes

Boundary / Edge 3:  The results (key/nothing/enemy) are repeated too often; the key is never generated.  
    \- Requirement(s): Variety of results; the key must be generated for the game to be playable.  
    \- Purpose: The results vary among them, but at some point the key is always generated.

    \- Test Case 1:  
        a) Upper Boundary: Repeated results  
        b) Testing data used: S“enemy” or “nothing” always appears  
        c) Expected Result / Behavior: A variety of items should be produced, using a limit of no more than 3 items.  
        d) Automation Candidate: Yes (high-volume)

    \- Test Case 2:  
        a) Lower Boundary: p(key)=0 always  
        b) Testing data used: Set random, the key is not generated  
        c) Expected Result / Behavior: Force key generation, don't make the game unplayable  
        d) Automation Candidate: Yes (high-volume)

\---

4\. Automated Test List  
Which tests were candidates for Automation?    
For each candidate, which kind of automated testing is recommended:    
repetitive, rule-based, or high-volume?

| Tests | Type of automated test |
| :---- | :---- |
| Basic movement | Repetitive |
| Invalid commands | Rule-Based |
| Wall collision detection | Repetitive |
| Map exit control | Repetitive |
| Key status | Rule-Based |
| Keyless door opening | Rule-Based |
| Event generation | High-Volume |
| p(key)=0 and repetition of events | High-Volume |

## 

