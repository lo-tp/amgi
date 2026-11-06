# The review session's total is the deck's live counts

A study session shows `position/total`, where `total` is the cards it has graded
plus the deck's remaining counts, re-read from the engine every time the queue is
re-gathered. Taking a card out of study — suspend, bury, delete, rescheduling it
past today — therefore shrinks that total mid-session, and a learning step coming
due or a sync landing cards can grow it. We chose the live figure because the
engine discards its study queue on any card change and derives the counts from
what it next gathers: any other total is a number the backend no longer agrees
with.

## Considered options

- **Fix the total to the queue the session started with**, so the bar never
  moves. Rejected: the number stops meaning "what is left in this deck", and it
  starts lying the moment anything other than an answer happens — the same class
  of bug that once left `remainingCounts` permanently adrift after a dropped
  answer.
- **Keep the removed cards in the denominator** as a "set aside" tally.
  Rejected: it invents a session-local concept the engine has no notion of, to
  account for a card that was deliberately parked, and the stats already count
  suspended cards where they belong.

## Consequences

- The progress bar can jump forward and the counter can rise mid-session. Both
  are the intended behaviour; do not "fix" them.
- A session never repairs its `cardQueue`. Anything that removes the current card
  goes through `ReviewSession.requeueAfterCardRemoval()`, which re-gathers.
- A card taken out of study is never counted as reviewed. Its departure is
  counted by identity (`cardsTakenOutOfStudyCount`), not from the totals, so the
  study page's caption can only claim cards that provably left — "Forget Card"
  sends its card straight back into the new queue.
