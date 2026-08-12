# shipcommit

Commit–reveal scheme for Battleship boards: each player commits to all 100
cells before the first shot, then reveals exactly the cells that get shot.
Nobody — not the opponent, not a relay, not a spectator — learns an unshot
cell, yet every reveal is verifiable against its commitment, so lying about
a hit is impossible.

Each commitment is SHA-256 over the deterministic CBOR of
`{cell, shipID, salt}`. Binding the cell index prevents replaying a
miss-reveal into a different square; committing the ship id (not a bare
occupied bit) makes "sunk" a public derivation — revealed hits of ship k
equal its length — so there is no sunk-declaration message to lie in.
Includes standard-fleet legality checking and legal random placement.

```go
board, commits, _ := shipcommit.NewBoard(placement) // board is SECRET
reveal := board.Cells[shot]                          // answer a shot
ok := shipcommit.Verify(commits[shot], reveal)       // anyone can check
```

Compiles to WASM. Extracted from
[kibitz](https://github.com/richardwooding/kibitz), the sibling of
[fairdice](https://github.com/richardwooding/fairdice).

MIT licensed.
