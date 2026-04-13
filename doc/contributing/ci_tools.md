# CI Tools {#ci_tools}

The following pages provide real-time visibility into the state of patches
submitted to the SPDK project:

* [Queue Status](https://review.spdk.io/queue_status.html) - Shows the current
  CI queue, including patches that are in progress and waiting to be tested.
  The page is updated every 60 seconds. The scheduler uses round-robin across
  patch owners so that no single contributor monopolizes the CI queue.

* [Mergable Changes](https://review.spdk.io/mergable_changes.html) - Shows
  the outstanding patch status, including changes ready for merge, changes
  needing another +2 Code-Review vote, and changes still awaiting review.
  The page is updated every 5 minutes.

- @subpage ci_overview
- @subpage shfmt
- @subpage distributions
