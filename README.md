# 📖 Word Dictionary

A developer's reference for programming terminology — clear definitions with memorable examples.

---

## Fixtures

In programming, **fixtures** refer to predefined data sets or configurations used for **testing**. They help create a consistent test environment by setting up known conditions before a test runs and cleaning up afterward.

### Common Uses

| Type | Description |
|------|-------------|
| **Unit & Integration Testing** | Set up isolated, repeatable test conditions |
| **Database Fixtures** | Seed a database with known data before tests run |
| **Frontend Testing (UI Fixtures)** | Simulate component states and user interactions |
| **Dependency Injection Fixtures** | Provide mock dependencies to the system under test |

---

## Substring

A **substring** is a smaller piece of a string made from consecutive characters in the original — like cutting a slice from a word without skipping any letters.

### Quick Rule
> Characters must be **contiguous** (no gaps) and in the **same order** as the original.

### Examples

```
"cat" is a substring of "concatenate"  ✅
 c-a-t appears right there, letter by letter, with no gaps
```

```
"I love coding"  →  "love coding" is a substring  ✅
"I love coding"  →  "I coding" is NOT a substring  ❌
                     (characters are not consecutive)
```


**Context**: https://leetcode.com/problems/longest-substring-without-repeating-characters/


---


## Subsequence

A **subsequence** is a smaller sequence of characters or elements taken from the original — in the same order, but **not necessarily consecutive** (gaps are allowed).

### Quick Rule
> Order must be **preserved**, but characters don't have to be **side by side**.

### Examples

```
"hlo" is a subsequence of "hello"  ✅
 h_l_o — same order, with gaps skipped
```

```
"hlo" is NOT a substring of "hello"  ❌
 because the characters are not consecutive
```

### Substring vs. Subsequence

| | Substring | Subsequence |
|---|---|---|
| Same order? | ✅ | ✅ |
| Must be consecutive? | ✅ | ❌ |
| Example from `"coding"` | `"cod"`, `"ing"` | `"cig"`, `"ong"` |

---

### Dependency Injection

### High Through

---

*Feel free to contribute definitions or examples!*
