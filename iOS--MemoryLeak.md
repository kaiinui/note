MemoryLeak
---

`CGImage` は ARC 効かないので `CGImageRelease()` で開放してやる
