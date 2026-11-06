# CONTEXT — the language of Amgi

Glossary only. No implementation, no decisions, no file names. When a term is
settled it is written here; when it is contested it is not.

## Study

- **Study session** — a run through one deck's cards, started from that deck. It
  ends when there is nothing left to study.
- **Card queue** — the ordered cards a session is working through. A queue is
  *re-gathered, never edited*: any change to a card discards it and the next
  gathering decides what comes next.
- **Remaining counts** — how many cards a deck would currently yield, split into
  new / learning / review. Suspended and buried cards are not in them.
- **Session total** — what the study page means by "how many cards this
  session": the cards already graded, plus the remaining counts. It is a live
  figure, so it can shrink (a card suspended or buried) or grow (learning cards
  come due, a sync brings cards) while the session runs.
- **Grade** — record a rating for a card. Grading is the only act that makes a
  card *reviewed*.
- **Reviewed count** — the cards actually graded in this session. Suspending,
  burying, marking, flagging or editing a card never adds to it.

## Taking a card out of study

- **Suspend** — put a card out of every study queue until someone restores it.
  Collection-wide: it holds across sessions, decks and days, and it survives in
  the browser, the deck's counts and the stats. The card keeps its own position,
  so restoring it returns it where it would have been. Suspending is never a
  grade and never makes a card reviewed.
- **Bury** — hide a card from study until the next day rolls over. Contrast with
  suspend: burying is temporary and self-reversing, suspending is deliberate and
  permanent until undone.
- **Note-level action** — an action aimed at a note applies to every card of that
  note, in every deck, at once. It can therefore take several cards out of a
  session in one step.
- **Re-queue** — what a session does after a card is taken out of study: the
  queue it was working through is discarded rather than repaired, the remaining
  counts and the session total are read again, and study continues with whatever
  the deck yields next. If nothing is yielded, the session is finished. A
  re-queue grades nothing and changes no card.
