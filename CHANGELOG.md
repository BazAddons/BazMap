# BazMap Changelog

## 026 — Silenced ADDON_ACTION_BLOCKED on map close

Closing the world map could trip a protected-call error
(`Button:SetPassThroughButtons()`) caught by BugSack / BugGrabber.
Same root cause as the previous tooltip / mouse-propagation taint
fixes: BazMap's `SetAttribute` calls on `WorldMapFrame` (required to
detach it from Blizzard's panel layout) taint the frame, and that
taint propagates into `MapCanvasPinMixin:CheckMouseButtonPassthrough`
when the map's hide cascade re-acquires every quest pin.
`CheckMouseButtonPassthrough` is now wrapped in the same pcall shim
the other tainted Blizzard methods use; the error is swallowed,
the map closes cleanly, and the affected pin loses correct
passthrough behaviour for that one frame only.
