### overview

this repository contains modifications to the voting mechanism for validators on solana. these changes introduce additional criteria for voting, aimed at optimizing vote timing and reducing the risk of getting stuck on dying forks. the modifications also include mechanisms for backfilling votes and preventing unnecessary vote expirations, which can improve vote credits but also introduce some induced lag under specific conditions.

### config
the configuration for these mods is stored in a file named mostly_confirmed_threshold, located in the validator's root directory. if the file doesn't exist, the mods do nothing. the file contains four values in sequence:
- mostly confirmed threshold: the fraction of stake-weighted votes required for a slot to be considered "mostly confirmed." for example, 0.55 means a slot is considered confirmed after receiving 55% of votes. higher values induce more conservative voting behavior.
- slots beyond confirmed slot: the number of slots beyond the most recent "mostly confirmed" slot that will be voted on regardless of stake weight. lower numbers result in more induced lag.
- skip behavior: set to 0, 1, or 2. it controls voting after a skip in slots. 0 disables additional processing. 1 or 2 adds further restrictions based on the "mostly confirmed threshold" or full consensus, respectively.
- escape hatch distance: number of slots without any votes before the mods temporarily disable themselves, acting as a safeguard against bugs or extreme cluster issues.

#### example config
  ```0.45 4 0 24```

  this config adds minimal lag, as the threshold is set relatively low and allows voting on four slots beyond the most recently confirmed slot.

### additional features
- backfill votes: votes are cast for any votable slots (e.g., B, C, D) if they follow a previously voted-on slot (e.g., A) but were not voted on at the time.
- vote expiration prevention: votes are no longer expired unnecessarily, leaving more votes in the tower and potentially earning more credits.
- vote pruning: limits the number of slots committed to a single fork beyond 64 slots to prevent excessive lockout.

### usage
use these mods on testnet first before deploying them on mainnet to ensure stability. be cautious with extreme values for the configuration, as they may cause voting issues. it is recommended to keep the mostly confirmed threshold below 0.6 and the number of vote-ahead slots below 4.

### notes
these mods are designed to enhance vote efficiency without altering core vote selection logic. validators still follow the stock solana fork-avoidance mechanisms, but additional safeguards are applied to increase vote credits and reduce the risk of voting on dead forks.

### future improvements

potential areas for further improvement include:
- refining safety mechanisms to avoid missing votes unnecessarily.
- enhancing fork-avoidance heuristics to better predict when a fork is likely to die.
- improving vote selection to prioritize faster or healthier forks.

### conclusion
this mod offers a balance between improving vote commitment and maintaining safety, with the ability to backfill missed votes and reduce unnecessary vote expirations. however, caution is advised when adjusting parameters, as aggressive configurations can disrupt voting.

# credits
thanks to zantetsu from shinobi systems <3
