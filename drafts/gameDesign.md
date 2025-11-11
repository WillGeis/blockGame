# Game Design Overview

## Game Mathematics

### Starting Conditions

The game border design will be defined by a 4 arrays (```Border```, ```InCuts```, ```Blocks```, ```Exits```) of integer arrays defined by this basic grid design (pictured below). The basic design of this array is blockID, x-coordinate, y-coordinate.

<img width="855" height="731" alt="image" src="https://github.com/user-attachments/assets/f23fd1bf-948c-4a18-b30e-53c7d6e7a2c8" />

```
[[blockID, x-coordinate, y-coordinate],[0,0,0,0],[0,0,20,20]]
```

This above definition would define a basic 20 x 20 grid. The block ID will vary in number of digits to capture the number of entities respectively (i.e. borders will be filled with a 0 or 1, incuts will be filled as a 2 digit integer, and blocks will have a 3 digit integer).

The first position of the ```Border``` array will be the block ID, border sub-arrays will be defaulted to 0 and 1 respectively (i.e. ```[[0,0,0],[1,n,m]]```). The coordinate system is what is pictured above.

The first position of the ```InCuts``` array will be the block ID, defaulted from 0-99 counting up from 0. The coordinate system is what is pictured above.

The first position of the ```Blocks``` array will be the block ID; for each block ID there will be 2 subarrays that correspond to it giving our bottom/top/left/right coordinates. Block sub-arrays will be assigned integers from 0-998 and scale with the size of the ```Border``` (i.e. ```av(Border[0][0] - Border[1][0]) * av(Border[0][1] - Border[1][1]) * .3 = numBlocks```). The coordinate system is what is pictured above.

The first position of the ```Exits``` array will be the block ID, defaulted from 0-99 counting up from 0. The coordinate system will be what is pictured below to reflect what side of the border the exit is defined at.

<img width="869" height="685" alt="image" src="https://github.com/user-attachments/assets/55c99cb0-ffe0-4c15-9390-3b3bb1e82b90" />

For now, these 4 arrays will be hardcoded and stored in Levels.csv

### Game Math (at runtime)

When a user clicks and drags a block, they are allowed to drag the block in the x or the y direction, as they do this the corresponding coordinate is incremented. Collision checks will happen with the other 3 arrays, and its own array in this way:

- ```Border``` if a blocks x or y coordinate equals the border coordinate the block will stop, this will be done by a "last feasible position" check, stopping or snapping (UPDATE WITH DESIGN) to the final feasible position
- ```Blocks``` if a blocks x or y coordinates stray into another block, the x and y coordinates of that block will not move
- ```InCuts``` same as ```Blocks```
- ```Exits``` if a block hits a border, it will call exits and check if the logic will allow for a block to be moved outside the border (i.e. if ```Exits[0] = [0,2,9]``` and ```Blocks[0] = [0,2,0]``` and a user decrements the y coordinate of ```Blocks[0]``` the block will disappear and set ```Blocks[0] = [999,0,0]```

### Game End

A game will end if ```Blocks[*][0] == 999```
