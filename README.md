# Ghost branch

Rewind + ghost-replay prototype. Hold Space to rewind, release to spawn a ghost that replays your previous trajectory. Ghost is non-solid while overlapping you, becomes solid once you separate, bursts into confetti at the end of its path. Death also bursts confetti and waits for a rewind instead of resetting.

## Known issues / open questions

- **Curbstomping main player with the clone.** Solid ghost can squash the live player against terrain (or push them into pits) with no recourse besides another rewind. Probably needs either: ghost can't apply lethal forces, or player can cancel a ghost on demand.
- **Riding the clone's head while jumping doesn't work right.** Carry logic kicks in only when `onGround` is true at the start of the frame; jumping off the ghost or landing on it mid-flight has rough edges (jitter, missed catches). Needs a proper "platform rider" pass that survives a frame of airtime.
- **Is this even fun?** Open question. Right now the ghost is mostly a curiosity — it doesn't unlock anything you couldn't already do. Need a level/mechanic that actually requires the clone to exist.
- **Probably needs switches / things outside of time.** Pure "you + past you" doesn't give the player puzzles to solve. Something the ghost can hold down (pressure plates), block (crushers), or trigger (one-shot doors) would make the rewind feel load-bearing instead of cosmetic.
