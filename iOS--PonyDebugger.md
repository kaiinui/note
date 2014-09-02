PonyDebugger
===

やばかった。

CoreData: https://vine.co/v/OBVFZpg7QaX

View Hierarchy: https://vine.co/v/OBVt1XZFYxd

Installation
---

https://github.com/square/PonyDebugger

With Real Device
---

`127.0.0.1` ではなく `0.0.0.0` でやる。

Code
---

```objc
    PDDebugger *debugger = [PDDebugger defaultInstance];
    [debugger enableCoreDataDebugging];
    [debugger addManagedObjectContext:[[CoreDataManager sharedManager] managedObjectContext] withName:@"My MOC"];
    [debugger enableNetworkTrafficDebugging];
    [debugger enableRemoteLogging];
    [debugger enableViewHierarchyDebugging];
    [debugger autoConnect];
```

こんなのを適当にぶち込む
