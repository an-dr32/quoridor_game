# GridPath: A Quoridor Clone — A Quoridor-Inspired Phaser Game

Welcome to **GridPath: A Quoridor Clone**, a digital reimagining of the classic strategy game **Quoridor**, built with [Phaser 3](https://phaser.io/phaser3) and wrapped in retro music, vibrant visuals, and challenging grid-based tactics.

## 🎮 Gameplay Overview

**GridPath: A Quoridor Clone** is a two-player strategy game where each player must navigate a pawn from their starting row to the opposite end of the grid. Along the way, players can place walls to block their opponent — but never completely trap them!

The game is played on a 9x9 grid, with each player having **10 walls** to use strategically.

---

## 📜 Rules

- **Objective**: Reach the opposite row before your opponent does.
  - Player 1 starts at the bottom row and must reach the top.
  - Player 2 starts at the top row and must reach the bottom.
- **Turn Structure**:
  - Players alternate turns.
  - Each turn, a player may either:
    - **Move their pawn** one tile (up, down, left, or right), or
    - **Place a wall** to block movement.
- **Wall Placement**:
  - Press `W` to enter wall placement mode.
  - Press `V` to switch wall orientation (horizontal or vertical).
  - Click a legal location to place a wall.
  - You **cannot completely block** a player's path — there must always be a way for both players to reach their goal.
- **Restrictions**:
  - You cannot move onto a tile occupied by your opponent.
  - Walls cannot overlap.
  - Wall placement is validated using a pathfinding check to ensure fairness.
- **Victory**:
  - The first player to reach their goal row wins.
  - A modal with restart option appears on victory.

---

## 🎮 Controls

| Action              		| Key/Button        			 |
|---------------------------|--------------------------------|
| Move Pawn            		| Click on highlighted tile 	 |
| Enter Wall Mode      		| `W`               			 |
| Switch Wall Orientation 	| `V`          					 |
| Place Wall          		| Click on ghost wall preview 	 |
| Change Music Track 		| `P`               			 |
| Mute/Unmute Music   		| `M`             				 |
| Restart Game        		| `ESC`             			 |

---

## ⚙️ Game Functionality Highlights

- Built in **Phaser 3** using a fully custom scene and grid logic.
- **Wall placement validation** using internal A* style path check to ensure players aren't trapped.
- **Smooth music handling** with track rotation, mute, and persistence across restarts.
- Ghost wall previews with collision checks.
- Highlighted tiles for movement feedback.
- Responsive UI: track names, turn indicators, goal rows colored with soft overlays.

---

## 🔊 Sound Design

- Background tracks are rotatable with `P`, giving each session a unique vibe.
- Custom sound effects for:
  - Moving pawns
  - Placing walls
  - Winning
  - Invalid moves (with anti-spam cooldown)

---

## ❗Challenging Parts of the Code

### 1. **Pathfinding for Legal Wall Placement**
To prevent players from trapping their opponent, we simulate the wall placement, then run a BFS-style pathfinding check from both players:

	hasPathToGoal(startX, startY, goalY)
	
This ensures each wall placement leaves at least one route to the opponent’s goal row.

###2. Ghost Wall Previews Without Overlapping Pawns**
Wall previews must render between cells, not on them — while avoiding overlap with existing walls and bounds:

		if (this.wallMap.has(key) || this.wallMap.has(adjacentKey)) {
 			 return; // can't place overlapping or outside bounds
		}
		
###3. Movement Highlight System Using Overlays**
Highlighting movement options without overwriting the board was tricky. We solved it by drawing semi-transparent rectangles on top of the grid and tracking valid move targets as:

		{ x: number, y: number, rect: Phaser.GameObjects.Rectangle }
This let us keep the goal row colors intact.

###4. Preventing Invalid Sound Spam**
We used a cooldown flag + Phaser's delayedCall to avoid spamming the invalid move sound:

		if (!this.invalidSoundCooldown) {
		  this.invalidSound.play();
		  this.invalidSoundCooldown = true;
		  this.time.delayedCall(300, () => { this.invalidSoundCooldown = false; });
		}

		
💻 Installation & Running Locally
1. Clone the repo:

		git clone https://github.com/your-username/quoridor_game.git
		cd quoridor_game
		
2. Open index.html in your browser (you can use a simple HTTP server like live-server).

👨‍💻 Author
Created by Andres DM AKA L0gg3r
Feel free to contribute, remix, or build your own version!

🐔 Bonus Lore of my other game:
	Why is it called Nuclear Chick-EN?
	Because in a post-apocalyptic future, chickens play mind games with walls of radioactive metal and classical music.


Default copyright laws, apply, meaning The owner of this repository retains all rights, to the source code and no one may reproduce, distribute, or create derivative works from this repository, without express consent.
