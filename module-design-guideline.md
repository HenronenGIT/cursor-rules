# Modules Design Guidelines

Good modules are deep: they expose a small, simple interface while hiding meaningful internal complexity. Prefer somewhat general-purpose abstractions that serve multiple real use cases instead of over-specialized APIs. In layered systems, each layer must provide a distinct abstraction — avoid pass-through wrappers and decorators that add structure but no reduction in complexity.

---

## Design modules to be deep
- Small, simple interface  
- Powerful, hidden implementation  
- Short, obvious call sites  

Avoid shallow modules:
- Callers compose multiple steps for one task  
- Public API mirrors internal structure  
- Little complexity is actually hidden  

---

## Core Principles

### 1. Start from use cases
Write 2–3 example call sites first.  
The common case should be **1 object + 1 method**.

---

### 2. Keep the interface small
- Few public methods (often 1–5)
- Clear defaults
- Minimal required concepts

If using the module requires learning many types, it’s too shallow.

---

### 3. Be somewhat general-purpose
Avoid over-specialization.

Ask:
- What is the simplest interface that covers all current needs?
- In how many situations will this method be used?
- Is this API easy to use for today’s main use case?

Generalize to the next 1–2 realistic uses — not everything.

Do not:
- Encode specific UI flows in method names
- Create one-off public methods for a single call site
- Add speculative configuration “just in case”

---

### 4. Different layer = different abstraction
Each layer must provide a **distinct abstraction**.

Red flags:
- Pass-through methods that only forward calls
- Adjacent layers exposing the same concepts
- Wrapper/decorator classes adding boilerplate but little value

If a class mostly delegates:
- Merge it with the underlying class, or  
- Move the new behavior into the existing abstraction  

Every layer must reduce complexity, not duplicate it.

---

### 5. Hide complexity aggressively
Inside the module:
- Handle edge cases
- Normalize inputs
- Centralize policy decisions
- Manage retries, caching, batching, etc.

Callers should not reimplement this logic.

---

### 6. Don’t leak internals
Public API must not expose:
- Storage or tech choices
- Internal stages
- Ordering requirements

If internals change, callers should not break.

---

## Quick Tests

- Can the common task be done in ≤2 calls?
- Does this module significantly reduce caller complexity?
- Is each public method used in multiple places?
- Does this layer provide a distinct abstraction?
- Would changing internals require changing callers?

If not, the module is likely shallow.
