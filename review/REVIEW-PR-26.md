# Review: price-table-block PR #26 - Keep the block icon visible when the editor highlights it

PR: https://github.com/EssentialBlocks/price-table-block/pull/26
Verdict: **Approve** (small rebase onto dev `6df38bb` suggested)

## What the PR does
Adds `assets/css/block-icon-selected.css` and a small `enqueue_block_editor_assets` hook in the main plugin file.
The block icon is filled `#6C3BFF`, which nearly disappears on the blue highlighted row of the `/` inserter and on a selected List View row.
The CSS recolours those fills to `currentColor` on those rows only.

## Real merge impact
GitHub may list more files, but a simulated merge into `dev` (`git merge-tree`) changes **2 files, +56**: the new CSS (40 lines) and the PHP hook (16 lines). No conflicts.
The CSS is byte-identical to the one in the Testimonial, Toggle Content and Typing Text icon PRs (same md5), and uses the same shared handle `eb-block-icon-selected` (version `1.0.0`), so it prints once when several of these plugins are active.

## Checks
- `php -l` on the merged PHP file: no syntax errors.
- Browser test, Local WordPress 7.1.3, Chrome 153, only this plugin active, "Pricing Table" block, `dev` vs the simulated merge:

| Place | Before | After |
|---|---|---|
| Highlighted `/` inserter row (row bg rgb(56,88,233)) | icon fill rgb(108,59,255) | rgb(239,240,242) |
| Selected List View row | rgb(108,59,255) | rgb(255,255,255) |
| Stylesheet loaded | no | yes |

Screenshots: `review/screenshots/pr26-before-slash.png`, `pr26-after-slash.png`, `pr26-before-listview.png`, `pr26-after-listview.png`.

## Notes
- Rebase onto current `dev` (`6df38bb`) before merging; the branch was cut from an older base.

## Not covered
- Chrome 153 only (no Firefox/Safari), PHP 8.2 only (the plugin supports 7.4), WordPress 7.1.3 on Local.
- The drag chip (third place the icon CSS targets) was not tested.
- The author had not tested this plugin in a browser before opening the PR; these are the first browser measurements.
