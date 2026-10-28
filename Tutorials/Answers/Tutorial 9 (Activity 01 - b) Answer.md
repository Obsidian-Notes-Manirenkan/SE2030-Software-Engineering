## 1. Most appropriate design pattern

**Observer Pattern** (Behavioral design pattern)

---

## 2. Justification

In the INavigator system:

- The **GPS class** continuously receives, parses, and stores new `$GPRMC` data (the **Subject / Observable**).
- There are **three different views** (Text View, Compass View, and Breadcrumb Trail View) that must display different aspects of the same data.
- Whenever new GPS data arrives, **all views must be updated automatically**.

The Observer pattern is ideal because:

- It establishes a **one-to-many dependency** between the GPS data source and the views.
- Views can **subscribe/unsubscribe** dynamically.
- The GPS class does **not need to know** the concrete details of each view — it simply notifies all registered observers.
- This keeps the system loosely coupled and easy to extend (e.g., adding a new map view later).

---

## 3. Simple Class Diagram (textual representation)

![[Pasted image 20261028131418.png]]

**Key relationships:**

- `GPS` maintains a list of `Observer` objects.
- When new GPS data is parsed, `GPS` calls `notify()`, which calls `update()` on every registered observer.
- Each concrete view implements `update()` and refreshes its display using the latest data from the GPS object.

This is the classic Observer pattern applied to the multi-view GPS navigation system.

---

[[Why won't the 'Strategy Design Pattern' suits this]]
