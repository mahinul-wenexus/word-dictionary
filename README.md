# 📖 Developer Word Dictionary
> A personal reference for programming terminology — clear definitions with memorable examples.

---

## Table of Contents

**🧪 Testing**
- [Fixtures](#fixtures)

**🔤 Strings & Sequences**
- [Substring](#substring)
- [Subsequence](#subsequence)

**🏗️ Design Patterns**
- [Dependency Injection](#dependency-injection)
- [Memoization](#memoization)
- [Circuit Breaker](#circuit-breaker)
- [Graceful Degradation](#graceful-degradation)

**⚡ Performance**
- [High Throughput](#high-throughput)
- [Caching](#caching)
- [Debouncing vs Throttling](#debouncing-vs-throttling)
- [Pagination](#pagination)
- [Backpressure](#backpressure)

**🔄 Concurrency & Systems**
- [Race Condition](#race-condition)
- [Deadlock](#deadlock)
- [Concurrency vs Parallelism](#concurrency-vs-parallelism)
- [Idempotency](#idempotency)

**🌐 Distributed Systems**
- [Eventual Consistency](#eventual-consistency)
- [Polling vs Webhooks](#polling-vs-webhooks)
- [Serialization and Deserialization](#serialization-and-deserialization)

---

# 🧪 Testing

---

## Fixtures

In programming, **fixtures** refer to predefined data sets or configurations used for **testing**. They help create a consistent environment by setting up known conditions before a test runs and cleaning up afterward.

### Quick Rule
> Set up the world before the test. Tear it down after.

### Common Uses

| Type | Description |
|------|-------------|
| **Unit & Integration Testing** | Set up isolated, repeatable test conditions |
| **Database Fixtures** | Seed a database with known data before tests run |
| **Frontend Testing (UI Fixtures)** | Simulate component states and user interactions |
| **Dependency Injection Fixtures** | Provide mock dependencies to the system under test |

---

# 🔤 Strings & Sequences

---

## Substring

A **substring** is a smaller piece of a string made from **consecutive** characters in the original — like cutting a slice from a word without skipping any letters.

### Quick Rule
> Characters must be **contiguous** (no gaps) and in the **same order** as the original.

### Examples

```
"cat" is a substring of "concatenate"  ✅
 c-a-t appears right there, letter by letter, with no gaps

"I love coding"  →  "love coding" is a substring  ✅
"I love coding"  →  "I coding" is NOT a substring  ❌
                     (characters are not consecutive)
```

**Context**: [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

---

## Subsequence

A **subsequence** is a smaller sequence of characters taken from the original — in the same order, but **not necessarily consecutive** (gaps are allowed).

### Quick Rule
> Order must be **preserved**, but characters don't have to be **side by side**.

### Examples

```
"hlo" is a subsequence of "hello"  ✅
 h_l_o — same order, with gaps skipped

"hlo" is NOT a substring of "hello"  ❌
 because the characters are not consecutive
```

### Substring vs Subsequence

| | Substring | Subsequence |
|---|---|---|
| Same order? | ✅ | ✅ |
| Must be consecutive? | ✅ | ❌ |
| Example from `"coding"` | `"cod"`, `"ing"` | `"cig"`, `"ong"` |

---

# 🏗️ Design Patterns

---

## Dependency Injection

**Dependency Injection (DI)** is a design pattern where a class or function **receives** its dependencies from the outside instead of creating them internally.

### Quick Rule
> Don't build what you need — **ask for it**.

### Example

```python
# ❌ Without DI — tightly coupled, hard to test
class OrderService:
    def __init__(self):
        self.db = MySQLDatabase()  # hardcoded dependency

# ✅ With DI — loosely coupled, easy to swap or mock
class OrderService:
    def __init__(self, db):        # dependency is injected
        self.db = db

service = OrderService(db=MockDatabase())  # easy to test!
```

### Why It Matters

| Without DI | With DI |
|------------|---------|
| Hard to test (real DB required) | Easy to inject mocks |
| Tightly coupled to one implementation | Swap implementations freely |
| Violates Single Responsibility Principle | Clean separation of concerns |

---

## Memoization

**Memoization** is an optimization technique where a function **caches its results** — if called again with the same inputs, it returns the stored result instead of recomputing.

### Quick Rule
> Remember the answer so you never solve the same problem twice.

### Example

```python
# ❌ Without memoization — recomputes every time
def fib(n):
    if n <= 1: return n
    return fib(n - 1) + fib(n - 2)   # fib(30) makes 2.7M calls 😬

# ✅ With memoization
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n <= 1: return n
    return fib(n - 1) + fib(n - 2)   # fib(30) makes 31 calls ✅
```

> **Context**: React's `useMemo`, `useCallback`, and `React.memo` are all memoization tools for avoiding unnecessary re-renders.

---

## Circuit Breaker

A **circuit breaker** stops calling a failing service after a threshold of errors — giving it time to recover instead of hammering it with more requests.

### Quick Rule
> If a service keeps failing, **stop trying** for a while. Then test if it's back.

### The Three States

```
CLOSED ──(too many errors)──▶ OPEN ──(timeout passes)──▶ HALF-OPEN
  ▲                                                           │
  └───────────(test request succeeds)────────────────────────┘
```

| State | Behavior |
|-------|----------|
| **Closed** | Normal — all requests pass through |
| **Open** | Failing — all requests are immediately rejected |
| **Half-Open** | Testing — one request let through to check recovery |

### Why It Matters
Without a circuit breaker, a slow downstream service causes a **cascade failure** — your whole system backs up waiting for responses that never come. The circuit breaker **fails fast** and protects the rest of your system.

---

## Graceful Degradation

**Graceful degradation** is the ability of a system to **keep functioning at a reduced level** when part of it fails — rather than crashing entirely.

### Quick Rule
> Something breaks → deliver **less**, not **nothing**.

### Example

```
🛒 E-commerce site — recommendation service goes down

❌ Without graceful degradation:
   → Entire page crashes with a 500 error

✅ With graceful degradation:
   → Page loads normally, recommendations show a fallback
     ("You might also like" is hidden, or shows top-selling items)
```

### Common Patterns

| Pattern | Description |
|---------|-------------|
| **Fallback UI** | Show a placeholder when a widget fails to load |
| **Cached response** | Return stale data instead of an error |
| **Feature flags** | Disable a failing feature without a redeploy |
| **Default values** | Return sensible defaults when a service is unavailable |

> **Progressive Enhancement** is the flip side — start with a minimal working base and *add* features on top, so the base always works.

---

# ⚡ Performance

---

## High Throughput

**High throughput** describes a system's ability to **process a large volume of operations** (requests, messages, transactions) in a given time period.

### Quick Rule
> Throughput = **how much** work gets done, not how fast a single task finishes.

### Throughput vs Latency

| | Latency | Throughput |
|---|---|---|
| Measures | Speed of **one** request | Volume of **many** requests |
| Unit | milliseconds (ms) | requests/second (RPS) |
| Goal | Fast individual response | Handle massive load |
| Example | A single API call takes 10ms | The server handles 50,000 RPS |

> 🛣️ **Highway analogy**: Latency is how fast one car travels. Throughput is how many cars pass through per hour.

---

## Caching

**Caching** stores the result of an expensive operation in a faster location so future requests can skip the work and retrieve it instantly.

### Quick Rule
> Compute once, **reuse many times**.

### Example

```
❌ Without cache:
   User requests a page → hits database → runs query → builds response
                                          (200ms every single time)

✅ With cache:
   First request    → hits database → stores result in Redis
   Next 10,000 reqs → returned from cache in 1ms ✅
```

### Cache Invalidation
The hardest problem in caching — knowing **when to throw the old data away**.

> *"There are only two hard things in Computer Science: cache invalidation and naming things."* — Phil Karlton

| Strategy | How it works |
|----------|-------------|
| **TTL (Time To Live)** | Cache expires automatically after N seconds |
| **Write-through** | Update cache every time you write to the DB |
| **Cache busting** | Force a fresh fetch by changing the resource key |

---

## Debouncing vs Throttling

Both techniques **limit how often a function runs**, but for different reasons.

### Debouncing
> Wait until the action **stops** before executing.

```
User typing: "r" → "re" → "rea" → "reac" → "react"
             ⏳ timer resets on each keystroke
                              → ONE API call fires after they stop ✅
```

### Throttling
> Execute **at most once** per time window, no matter how many triggers.

```
User scrolling rapidly → 200 scroll events/sec
With throttle(100ms)   → only 10 function calls/sec ✅
```

### Side-by-Side

| | Debounce | Throttle |
|---|---|---|
| Fires when? | After activity **stops** | At **regular intervals** |
| Best for | Search inputs, form validation | Scroll, resize, mouse move |
| Guarantees execution? | Only after a pause | Yes, periodically |

---

## Pagination

**Pagination** splits a large data set into **smaller chunks (pages)** so the server doesn't return everything at once.

### Quick Rule
> Never send 10 million rows when the user only sees 20 at a time.

### Two Common Strategies

```
Offset Pagination — skip N rows, take the next M
  GET /posts?page=3&limit=20
  → SELECT * FROM posts LIMIT 20 OFFSET 40
  Simple, but slow on large tables (DB still scans all skipped rows)

Cursor Pagination — start from where you left off
  GET /posts?cursor=eyJpZCI6MTAwfQ&limit=20
  → SELECT * FROM posts WHERE id > 100 LIMIT 20
  Fast at any depth, but you can't jump to an arbitrary page
```

| | Offset | Cursor |
|---|---|---|
| Jump to page N? | ✅ | ❌ |
| Performance at scale | ❌ Degrades | ✅ Consistent |
| Best for | Admin dashboards | Infinite scroll / feeds |

---

## Backpressure

**Backpressure** lets a **slower consumer signal to a faster producer** to slow down — preventing the consumer from being overwhelmed.

### Quick Rule
> The receiver says *"slow down, I can't keep up"* — and the sender listens.

### Example

```
🚰 Producer generates 10,000 events/sec
📦 Consumer can only process  1,000 events/sec

❌ Without backpressure → queue grows → memory explodes → crash
✅ With backpressure   → producer slows to 1,000 events/sec → stable
```

### Real-World Appearances

| Context | How backpressure shows up |
|---------|--------------------------|
| **TCP** | Receiver's buffer fills → sender slows automatically |
| **RxJS / Reactive Streams** | Consumers request only as many items as they can handle |
| **Kafka** | Consumers pull at their own pace instead of being pushed to |
| **Node.js Streams** | `pipe()` handles backpressure automatically |

---

# 🔄 Concurrency & Systems

---

## Race Condition

A **race condition** occurs when two or more processes access shared data **at the same time**, and the final result depends on the **unpredictable order** in which they execute.

### Quick Rule
> When the outcome depends on **who gets there first** — that's a race condition.

### Example

```
💰 Bank account balance: $100

Thread A reads balance → $100
Thread B reads balance → $100
Thread A adds $50  → writes $150
Thread B adds $50  → writes $150   ❌ Should be $200!

Both threads read the old value before either could write back.
```

### Common Fixes

| Fix | Description |
|-----|-------------|
| **Mutex / Lock** | Only one thread accesses the resource at a time |
| **Atomic operations** | Read-modify-write as a single uninterruptible step |
| **Optimistic locking** | Check if data changed before committing a write |

---

## Deadlock

A **deadlock** is when two or more processes are **stuck waiting for each other** indefinitely — none can proceed because each holds a resource the other needs.

### Quick Rule
> Process A waits for B. Process B waits for A. **Nobody moves.**

### Example

```
🔒 Thread A holds Lock 1, waiting for Lock 2
🔒 Thread B holds Lock 2, waiting for Lock 1

→ Both threads wait forever ❌
```

### The 4 Conditions for Deadlock (all must be true)

| Condition | Meaning |
|-----------|---------|
| **Mutual Exclusion** | Resources can't be shared |
| **Hold and Wait** | A process holds one resource while waiting for another |
| **No Preemption** | Resources can't be forcibly taken away |
| **Circular Wait** | A circular chain of processes each waiting on the next |

> Break **any one** condition and the deadlock can't form.

---

## Concurrency vs Parallelism

These are often used interchangeably, but they mean very different things.

### Concurrency
> **Dealing with** multiple tasks at once — tasks take turns sharing resources.

```
One chef 🧑‍🍳 — chops vegetables, stirs the pot, chops more.
Tasks are interleaved. Only one thing happens at a literal instant.
```

### Parallelism
> **Doing** multiple tasks at once — tasks literally run simultaneously.

```
Two chefs 🧑‍🍳🧑‍🍳 — one chops, one stirs. At the same time.
Requires multiple CPU cores.
```

### Side-by-Side

| | Concurrency | Parallelism |
|---|---|---|
| Truly simultaneous? | ❌ (interleaved) | ✅ |
| Requires multiple cores? | ❌ | ✅ |
| Goal | Responsiveness | Raw speed |
| Example | Node.js event loop | Multi-threaded video encoding |

> Rob Pike (Go's creator): *"Concurrency is about **structure**. Parallelism is about **execution**."*

---

## Idempotency

An operation is **idempotent** if calling it **multiple times produces the same result** as calling it once — no extra side effects.

### Quick Rule
> Doing it once = doing it a hundred times. **Same outcome, always.**

### Examples

```
PUT /users/42  { "name": "Alice" }   ✅ Idempotent
→ Run it 10 times — the user's name is still "Alice"

POST /orders  { "item": "Book" }     ❌ NOT Idempotent
→ Run it 10 times — you get 10 duplicate orders
```

### Why It Matters
Idempotency is critical in distributed systems — **network retries** happen all the time. If your API isn't idempotent, a retry can create duplicate payments, duplicate records, or corrupt state.

| HTTP Method | Idempotent? | Safe? |
|-------------|-------------|-------|
| `GET` | ✅ | ✅ |
| `PUT` | ✅ | ❌ |
| `DELETE` | ✅ | ❌ |
| `POST` | ❌ | ❌ |

---

# 🌐 Distributed Systems

---

## Eventual Consistency

In distributed systems, **eventual consistency** means all nodes will **converge to the same data** — but not necessarily right away. For a window of time, different nodes may return different values.

### Quick Rule
> The data will be consistent **eventually** — just not guaranteed **immediately**.

### Example

```
You post a photo on Instagram 📸
→ Your friend nearby sees it instantly          ✅
→ Your friend across the world sees it 2s later ⏳
→ Both see the same photo eventually            ✅
```

### Eventual vs Strong Consistency

| | Strong Consistency | Eventual Consistency |
|---|---|---|
| All nodes agree? | ✅ Immediately | ⏳ Eventually |
| Performance cost | High (coordination overhead) | Low |
| Best for | Banking, payments | Social feeds, DNS, caches |
| Example | Traditional SQL DB | DynamoDB, Cassandra |

> This is one of the core trade-offs in the **CAP theorem** — you can't have perfect consistency *and* high availability at the same time in a distributed system.

---

## Polling vs Webhooks

Both are ways to **get updated data from a server**, but they work in opposite directions.

### Polling
> **You** keep asking the server — *"Is there anything new?"*

```
Client → "Any new messages?" → Server: "No"   ⏳
Client → "Any new messages?" → Server: "No"   ⏳
Client → "Any new messages?" → Server: "Yes!" ✅
```

### Webhooks
> **The server** tells you when something happens — no asking required.

```
Server → "Hey, a new message just arrived!" → Client ✅
         (fires instantly, only when needed)
```

### Side-by-Side

| | Polling | Webhooks |
|---|---|---|
| Who initiates? | Client | Server |
| Efficiency | ❌ Wastes requests | ✅ Only fires on events |
| Setup complexity | Simple | Requires a public endpoint |
| Best for | Simple / legacy systems | Real-time event-driven systems |
| Example | Checking email every 30s | Stripe notifying your app of a payment |

---

## Serialization and Deserialization

**Serialization** converts an in-memory object into a format (like JSON or binary) that can be **stored or transmitted**. **Deserialization** is the reverse — turning it back into a usable object.

### Quick Rule
> Serialize = **object → bytes**. Deserialize = **bytes → object**.

### Example

```python
import json

user = {"name": "Alice", "age": 30}

# Serialization — object into a transmittable string
payload = json.dumps(user)    # '{"name": "Alice", "age": 30}'

# Deserialization — string back into a usable object
parsed = json.loads(payload)  # {"name": "Alice", "age": 30}
print(parsed["name"])         # "Alice" ✅
```

### Common Formats

| Format | Use Case |
|--------|----------|
| **JSON** | Web APIs, config files |
| **XML** | Legacy systems, SOAP APIs |
| **Protocol Buffers** | High-performance internal services |
| **MessagePack** | Binary JSON — smaller and faster |

---

*Feel free to contribute definitions or examples!*
