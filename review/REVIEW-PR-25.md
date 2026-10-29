# Review: price-table-block PR #25 - Replace the deprecated DOMNodeInserted listener

PR: https://github.com/EssentialBlocks/price-table-block/pull/25
Verdict: **Approve** (small rebase onto dev `6df38bb` suggested)

## What the PR does
Replaces the deprecated `DOMNodeInserted` listener in `assets/js/eb-animation-load.js` with one capture-phase `change` listener on `document` that checks `event.target.id === 'eb-animation-style'`.
Chrome removed mutation events in v127, so on `dev` the old admin handler never runs and choosing an animation in the inspector does not swap the `eb___*` classes.

## Real merge impact
The PR page can show up to 56 changed files, because the branch was cut from other work that was later merged. A simulated merge into `dev` (`git merge-tree`) changes **1 file, +7/-11** (`assets/js/eb-animation-load.js`). No conflicts.

## Checks
- `node --check` (and `php -l` where PHP changed): no syntax errors.
- Browser test, Local WordPress 7.1.3, Chrome 153, only this plugin active. The PR's JS file was served in place of the plugin's file for the "PR merged" column; for accordion-toggle the PHP was also taken from the merge.

| Check | `dev` | PR merged |
|---|---|---|
| `DOMNodeInserted` listeners registered in the editor | 1 | 0 |
| Injected `eb___animated eb___fadeIn` after select `change` | stays `eb___...` (handler never ran) | becomes `eb__animated eb__fadeIn` |
| Console errors / page errors | none | none |

The "select change" check injects a block element and a `#eb-animation-style` select after page load, then fires `change` (a synthetic select, not the real Inspector UI).

## Notes
- Rebase onto current `dev` (`6df38bb`) before merging.

## Not covered
- Chrome 153 only (no Firefox/Safari), PHP 8.2 only (the plugin supports 7.4), WordPress 7.1.3 on Local.
- The drag chip (third place the icon CSS targets) was not tested.
- The author had not tested this plugin in a browser before opening the PR; these are the first browser measurements.
