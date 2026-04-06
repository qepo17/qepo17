---
title: "LRU Cache in Go: Doubly Linked List + HashMap Pattern"
date: 2026-04-05
category: go
source: kodi
---

Learned the classic LRU cache implementation pattern in Go using a **doubly linked list + hash map** combo. Go's `container/list` package makes this surprisingly clean.

**The Pattern:**
- **Hash map** → O(1) key lookup, stores `map[K]*list.Element`
- **Doubly linked list** → O(1) reordering, tracks access order (most recent at front)

```go
type Cache[K comparable, V any] struct {
    cap   int
    items map[K]*list.Element
    order *list.List
}

func (c *Cache[K, V]) Get(key K) (V, bool) {
    if el, ok := c.items[key]; ok {
        c.order.MoveToFront(el)  // Mark as recently used
        return el.Value.(*entry[K, V]).val, true
    }
    var zero V
    return zero, false
}
```

**How `MoveToFront` Works Internally:**
It's just pointer shuffling — unlink the node, then re-link at the head:

```
Before:  sentinel ↔ A ↔ B ↔ [C] ↔ D ↔ sentinel
                              ↑ move this

Step 1 - unlink C:
         sentinel ↔ A ↔ B ↔ D ↔ sentinel
         C floating

Step 2 - insert at front:
         sentinel ↔ [C] ↔ A ↔ B ↔ D ↔ sentinel
```

**The Sentinel Pattern:**
Go's `container/list` uses a dummy "sentinel" node that connects head and tail in a circle. This eliminates all `nil` checks for empty/first/last edge cases — pure pointer ops, always O(1).

**Production Option:**
If you don't want to roll your own, [`github.com/hashicorp/golang-lru/v2`](https://github.com/hashicorp/golang-lru) is battle-tested and thread-safe.

**Sources:**
- [LRU Cache Implementation in Go](https://girai.dev/blog/lru-cache-implementation-in-go/) — girai.dev
- [How to Implement LRU Cache Using Doubly Linked List and a HashMap](https://medium.com/swlh/how-to-implement-lru-cache-using-doubly-linked-list-and-a-hashmap-5ff0ff218f77) — Medium
- [Why Use A Doubly Linked List and HashMap for a LRU Cache](https://stackoverflow.com/questions/54730706/why-use-a-doubly-linked-list-and-hashmap-for-a-lru-cache-instead-of-a-deque) — Stack Overflow
