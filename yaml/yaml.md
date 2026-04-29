# 📄 YAML Notes

> **YAML** — *Yet Another Markup Language* (original backronym) / *YAML Ain't Markup Language* (recursive backronym, now preferred)

---

## 🤔 What is YAML?

- A **data format** used to exchange data between systems
- Similar to **XML** and **JSON**, but more human-readable
- In YAML, you can store **only data — not commands**
- Used heavily in **configuration files** (Docker, Kubernetes), logs, caches, and more

---

## 🔄 Data Serialization

**Data Serialization** is the process of converting complex data objects into a stream of bytes that can be stored or transmitted.

```
           Serializer
OBJECT ──────────────▶ Stream of Bytes ──▶ YAML / Database / Memory
  │
  └─▶ (contains both code + data)
```

The reverse process — converting a byte stream back into an object — is called **Deserialization**.

**Common Data Serialization Languages:** JSON, YAML, XML

### ✅ Benefits of YAML

- Very easy to **read and write** (human-friendly)
- **Strict syntax** — indentation is significant (like Python)
- Easily **convertible** to JSON or XML
- Supported by **most programming languages**
- More **expressive** when representing complex/nested data
- Rich ecosystem of **parsers and tools**

---

## 📐 Syntax Rules

- Uses **indentation** (spaces, not tabs) to define structure
- Comments start with `#`
- Key-value pairs are separated by `: `

---

## 🔤 Data Types

### Strings

```yaml
# Plain string
myself: Soumadip Sen

# Quoted strings (useful when string contains special chars)
fruit: "apple"
job: "swe"
```

#### Block Scalars

```yaml
# Literal block ( | ) — preserves newlines, great for multi-line text
bio: |
  hey my name is Soumadip, I am r*cist, S*xist

# Folded block ( > ) — folds newlines into spaces → single line output
message: >
  this will all be in one single line

# Equivalent explicit type
msg: !!str this will be in one single line
```

> 💡 Use `|` when you want line breaks preserved (e.g. scripts, poems). Use `>` when it's logically one sentence split across lines for readability.

---

### Numbers

```yaml
number: 573           # Integer
marks: 98.76          # Float

# Explicit integer types using !!int tag
zero: !!int 0
positiveNum: !!int 45
negativeNum: !!int 0b11001   # Binary  → 25
octalNum: !!int 0657         # Octal   → 431
hexa: !!int 0x45             # Hex     → 69
commaValues: !!int +540_000  # 540,000 (underscores as thousand separators)

# Exponential notation
"exponential numbers": 6.023E56  # Keys with spaces must be quoted
```

> 💡 YAML supports binary (`0b`), octal (`0`), and hexadecimal (`0x`) prefixes for integers — handy for config values like permissions or color codes.

#### Floats

```yaml
marks: !!float 56.89
infinite: !!float .inf    # Infinity
"not a num": .nan         # NaN (Not a Number)
```

---

### Booleans

```yaml
boolean: !!bool No   # false

# Accepted values for FALSE: no, n, N, false, False, FALSE
# Accepted values for TRUE:  yes, y, Y, true, True, TRUE
```

> ⚠️ YAML 1.1 (used by many tools) treats `yes/no/on/off` as booleans. YAML 1.2 only accepts `true/false`. Use `!!bool` explicitly to be safe.

---

### Null

```yaml
surname: !!null Null   # Accepted: null, Null, NULL, ~
```

---

### Dates & Timestamps

```yaml
date: !!timestamp 2002-12-14
"india time": 2001-12-15T02:59:43.10+5:30    # ISO 8601 with timezone offset
"no time zone": 2001-12-15T02:59:43.10       # UTC assumed
```

---

## 📋 Collections

### Sequences (Lists)

```yaml
# Block style
student: !!seq
  - marks
  - name
  - roll_no

# Flow style (inline)
cities: [newDelhi, mumbai]
```

#### Sparse Sequences (with empty/null values)

```yaml
"sparse seq":
  - hey
  - how
  -           # empty value
  - Null
  - sup
```

#### Nested Sequences

```yaml
-
  - mango
  - apple
  - banana
-
  - marks
  - roll num
  - date
```

---

### Maps (Key-Value Pairs)

Maps are the basic key: value building blocks of YAML.

```yaml
# Nested mapping (map within a map)
name: Kunal Kushwaha
role:
  age: 78
  job: student

# Equivalent inline (flow) style
role1: { age: 78, job: student }
```

---

### Pairs (Duplicate Keys Allowed)

```yaml
# !!pairs allows the same key to appear multiple times
# Result: an array of hash tables
"pair example": !!pairs
  - job: student
  - job: teacher
```

> 💡 Normal YAML maps don't allow duplicate keys. Use `!!pairs` when you explicitly need them (e.g. HTTP headers).

---

### Sets (Unique Values Only)

```yaml
# !!set ensures all values are unique
names: !!set
  ? Kunal
  ? Soumadip
  ? Tejas
```

> 💡 In a set, items are written with `?` (the key indicator) and have no value.

---

### Ordered Maps (`!!omap`)

```yaml
# !!omap preserves insertion order (unlike regular maps which may not)
People: !!omap
  - Soumadip:
      name: Soumadip
      age: 78
      height: 678
  - Rahul:
      name: Rahul OP
      age: 86
      height: 550
```

---

## ⚓ Anchors & Aliases (Reusability)

Anchors let you **define a block once** and **reuse it** elsewhere — like variables.

```yaml
# Define an anchor with &
likings: &likes
  "fav fruit": mango
  dislikes: grapes

# Reference it with *
person:
  name: Soumadip Sen
  <<: *likes           # Merge all keys from &likes
  dislikes: berries    # Override a specific key from the anchor
```

**Result for `person`:**
```yaml
name: Soumadip Sen
fav fruit: mango
dislikes: berries      # overrides the anchor's value of "grapes"
```

> 💡 `<<:` is the **merge key** — it injects all key-value pairs from the referenced anchor. Any keys defined after it in the same map will **override** the anchored values.

---

## 🧠 Quick Reference

```
Data Types       : str, int, float, bool, null, timestamp
Number bases     : 0b (binary), 0 (octal), 0x (hex), _ (thousands separator)
Block scalars    : | (literal/newlines), > (folded/single line)
Collections      : sequences (lists), maps (dicts)
Special types    : !!pairs, !!set, !!omap
Reusability      : & (anchor), * (alias), <<: (merge key)
Null values      : null, Null, NULL, ~
Booleans         : yes/no, true/false, on/off (YAML 1.1)
Comments         : # this is a comment
```
