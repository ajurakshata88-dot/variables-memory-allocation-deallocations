# Variables and Memory Management

This guide describes how variables are used and how Python and Node.js manage the memory associated with their values.

## Contents

- [1. Variables in Python](#1-variables-in-python)
	- [Uses and memory association](#uses-and-memory-association)
	- [Scope, lifetime, and deletion](#scope-lifetime-and-deletion)
- [2. Memory allocation in Node.js](#2-memory-allocation-in-nodejs)
- [3. Memory allocation and deallocation in Python](#3-memory-allocation-and-deallocation-in-python)

## 1. Variables in Python

### Uses and memory association

Variables give values names. Programs use them to store data, perform calculations, pass values to functions, and keep references to objects for later use.

In Python, assigning a value binds a name to an object; the name is not a box that permanently contains the value. Assigning one name to another normally creates another reference to the same object, rather than a copy:

```python
items = ["apple"]
also_items = items
also_items.append("pear")
print(items)  # ["apple", "pear"]
```

Names are associated with scopes, such as a function or module. The object they refer to is available as long as it can still be reached through live references.

### Scope, lifetime, and deletion

A local name normally stops being available when its function call ends. The object it referred to may live longer if another name, a collection, or a closure still refers to it. Therefore, the end of a variable's scope does not always mean that the object's memory is reclaimed at that exact time.

In CPython, reference counting commonly reclaims an object when its reference count reaches zero. A cyclic garbage collector can reclaim unreachable groups of objects that refer to one another. Other Python implementations may manage memory differently.

- `del name` removes that binding; it does not guarantee that an object is immediately destroyed if other references remain.
- When an object is no longer reachable, the runtime can reclaim its memory.
- Reclaimed memory may be reused by Python instead of immediately being returned to the operating system.
- Use explicit resource management, such as `with` for files, to close external resources promptly. This is separate from reclaiming object memory.

## 2. Memory allocation in Node.js

Node.js runs JavaScript on the V8 engine by default. A JavaScript variable is a binding to a value, which can be a primitive or an object. The engine decides how to represent values internally; it is not correct to assume every variable is always stored in a stack slot or every value in the same kind of memory.

Objects and other dynamically managed data are generally stored in the JavaScript heap. V8's garbage collector can reclaim objects that are no longer reachable from active program state. Collection is automatic, and code should not depend on exactly when it runs.

```javascript
function createRecord() {
	const record = { status: "ready" };
	return record;
}

const currentRecord = createRecord();
```

After `createRecord` returns, its local binding ends, but the returned object remains reachable through `currentRecord`.

- A closure, global variable, or collection can keep an object reachable after the function that created it has returned.
- When no live references reach an object, garbage collection can reclaim it; this does not guarantee an immediate drop in process memory or an immediate return of memory to the operating system.
- Node.js also uses memory outside the JavaScript heap, including memory used by some buffers and native components.
- `const` and `let` control binding reassignment and scope; they do not manually allocate or free object memory.

## 3. Memory allocation and deallocation in Python

Python allocates objects as values are created and binds variable names to those objects. Object layout and allocation strategies depend on the Python implementation. In CPython, reference counts are used for many objects, and its allocator reuses memory for many small allocations.

When references to an object disappear, CPython can usually deallocate it when its reference count reaches zero. Reference counting alone cannot collect unreachable reference cycles, so CPython also has a cyclic garbage collector. This behavior is specific to CPython; Python as a language does not require this exact strategy.

- Rebinding a name, for example `value = new_value`, removes that name's reference to its previous object. Other references can keep the old object alive.
- `del value` removes the name binding; it is not a command to force memory back to the operating system.
- Memory freed by an object may remain in Python's allocator for reuse, so process memory usage may not immediately decrease.
- For files, sockets, and similar resources, use their context managers or explicit close methods instead of relying on garbage collection timing.

In short, Python and Node.js both manage ordinary object memory automatically. In both, reachability determines whether an object is still needed, but the exact collection time and when memory is returned to the operating system are not guaranteed by ordinary variable assignment or deletion.