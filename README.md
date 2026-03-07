# MCP Chess Server

This MCP let's you play chess against any LLM.

## Installation

To use this chess server, add the following configuration to your MCP config:

```json
{
  "mcpServers": {
    "chess": {
      "command": "uvx",
      "args": [
        "mcp-chess"
      ]
    }
  }
}
```

## Usage

Play a game:
![Chess starting position with black pieces](screenshots/start_black.png)

![A few moves later](screenshots/acc_dragon.png)

Find a position in a PGN for game analysis:

![Find a position in a PGN](screenshots/find_position.png)

## Available Tools

The server provides the following tools:

*   `new_game(user_plays_white: bool = True)`: Starts a new game, resetting the board. By default, the user plays white. Sets the user's color for board orientation.
*   `make_move(move_san: str)`: Makes a move on the board using Standard Algebraic Notation (SAN) (e.g., 'e4', 'Nf3', 'Bxe5'). Returns the move in SAN and UCI, the new board FEN, and game status.
*   `get_engine_move(difficulty: str = "normal")`: Gets Stockfish's best move for the current position. Difficulty controls engine strength (see below).
*   `get_board_visualization()`: Provides the current state of the chessboard as an image. The board orientation automatically flips based on the user's assigned color.
*   `get_turn()`: Indicates whose turn it is ('white' or 'black').
*   `get_valid_moves()`: Lists all legal moves for the current player in UCI notation (e.g., 'e2e4', 'g1f3'). Returns an empty list if the game is over.
*   `get_fen()`: Returns the current board position as a FEN string.
*   `set_fen(fen: str, user_plays_white: bool = True)`: Sets the board to a specific FEN position. Useful for restoring saved games.
*   `find_position_in_pgn(pgn_string: str, condition: str)`: Finds the first board position in a PGN string matching a condition (e.g., "bishop on a3") and returns an image of that board state.

## Stockfish Integration

Requires [Stockfish](https://stockfishchess.org/) installed (`brew install stockfish` on macOS).

| Difficulty | Method | Strength | Think Time |
|------------|--------|----------|------------|
| `easy` | UCI_LimitStrength + ELO 1320 | ~beginner | 0.1s |
| `normal` | UCI_LimitStrength + ELO 1500 | ~club player | 0.3s |
| `hard` | UCI_LimitStrength + ELO 2200 | ~master | 0.5s |
| `ultra` | Skill Level 20 (unlimited) | ~3500+ ELO | 1.0s |

Set `STOCKFISH_PATH` env var if not at `/usr/local/bin/stockfish`.

