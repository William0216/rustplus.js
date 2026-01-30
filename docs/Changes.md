# Changes

This document records local deviations from upstream `rustplus.js` and why
they were made, so we can revisit them if Facepunch or upstream changes.

## 2026-01-30

### Proto3 migration to avoid decode mismatches

We migrated `rustplus.proto` from `proto2` to `proto3` semantics and removed
all `required` field labels. This avoids protobuf decode errors when servers
omit fields that previously were required (default values are now allowed to
be absent without a protocol error).

Key outcomes:
- `AppEntityPayload.Item.itemIsBlueprint` no longer throws when missing,
  because it is no longer `required`.
- Added `Reserved = 0` enum members for `AppEntityType` and
  `AppCameraRays.EntityType`, matching proto3 expectations.
- `AppEntityPayload.value`, `capacity`, and `protectionExpiry` are now plain
  proto3 fields (no `optional`), consistent with proto3 defaults.

### Prior fix: `queuedPlayers` decode mismatch

Earlier, `AppInfo.queuedPlayers` was changed to `optional` (with a note in the
proto) because some servers omit it. This avoids decode errors when the field
is missing and is kept for compatibility.

## Sources

- Issue about missing `queuedPlayers` in upstream:
  https://github.com/liamcottle/rustplus.js/issues/76
- Upstream proto reference used for comparison:
  https://github.com/elinkev/rustplus.js/blob/master/rustplus.proto
- PR discussing proto changes in `rustplus.js`:
  https://github.com/liamcottle/rustplus.js/pull/77
- PR with a patch-package based workaround in `rustplusplus`:
  https://github.com/alexemanuelol/rustplusplus/pull/412
