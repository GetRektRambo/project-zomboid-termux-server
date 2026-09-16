# Ghost Lessons — Receipt Discipline

Operations that lied about what they did. Each cost real debugging time;
none will cost yours. Every line in this table is a scar we earned so
your build wouldn't bleed the same way.

| Ghost | Lie | Truth | Receipt |
|-------|-----|-------|---------|
| `pm uninstall` multi-arg | "Failed entirely" | Partially executed anyway | manifest diff before/after |
| `pm` success token | "Failed" (no Success printed) | It worked — newer pm prints nothing | `pm list packages` diff |
| `adb connect` | "Connected" | Dead port | `adb devices` state |
| Paste hang | Commands "not running" | Open quote hangs bash at `>` | prompt check (`$` vs `>`) |
| sed literal-string edit | "Applied" | Variable-built flags match nothing | `grep -n` after edit |
| `tr -d [:space:]` | safe cleanup | eats newlines too | `tr -d ' \t\r'` explicitly |
| locale-sorted diff | "trees differ" | sort order differs | `LC_ALL=C sort` |

## The pattern

1. Query actual state, not the logged/claimed state
2. Count it (`wc -l`, `sha256sum`, `grep -c`) — receipts are numbers
3. Before/after diffs beat single snapshots
4. Unverified instructions are unexecuted instructions

## Golden-copy habit

Archive before surgery. Every risky change in this project survived
because rollback was one command away. A backup you don't need costs
nothing. A backup you wish you had when you're staring at a corrupted
world save costs everything.
