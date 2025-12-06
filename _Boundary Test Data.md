Logical Point A: Player Movement

| Boundary Input | Test Data | Expected Outcome |
| :---- | :---- | :---- |
| Off-map movement | Player at (0,0) attempts left move | It doesn't move, it displays "You can't leave the map" |
| Movement towards a wall | Facing a wall at (2,1), player at (1,1), player tries right | It doesn't go through the wall. The position doesn't change. |
| Invalid command | The player types "m0ve" or "move up up". Send more than 20 commands in 1 second | Invalid command. No movement occurs. Process commands at speed limit, do not duplicate commands |

|  |
| :---- |

Logical Point B: Key Status

| Boundary Input | Test Data | Expected Outcome |
| :---- | :---- | :---- |
| Pick up the key when you already have one | player.has\_key \= True, then returns to the cell and "picks up" another key | \`player.has\_key\` remains \`True\`, the counter does not increment. The message "You already have the key" is displayed. |
| Open the door without the key | Player.has\_key \= False, player tries to open the door | Display “You need the key” incomplete game, do not finish game |
| Invalid item status | When loading a game, player.has\_key \= “yes” or null | Convert the value to boolean, set to False |

Logical Point C: Random generation and encounters (enemies/key/nothing)

| Boundary Input | Test Data | Expected Outcome |
| :---- | :---- | :---- |
| p(key)=0 | Adjust random number generation so that the key is not generated | Detect “No key”, force key generation, do not make the game unplayable |
| An enemy always appears | The event is always in “enemy” mode | Add a limit to the repetition of events, such as not repeating more than 3 times. |
| Repeated results | One move is made in a row and it always generates nothing | Reviewing randomly should produce a variety of items |

# Reflection:

- The logical point of random generation and the possibility that the key will not be generated feels more fragile, since this could make the level impossible or the game unplayable.

- I will need to normalize type generation when loading key state, control the range before assigning positions, and verify random generation.

- I was surprised by the borderline case where the key can appear as null in the inventory, since that usually breaks simple logic if it's not validated.

