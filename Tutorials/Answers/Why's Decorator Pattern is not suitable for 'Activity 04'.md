The **Decorator** pattern is used when you want to **add extra responsibilities or behavior** to an **already existing object** dynamically (by wrapping it), without changing its class.

Typical use cases of Decorator:
- Adding borders, scrollbars, or shadows to a window
- Adding toppings to a pizza
- Adding gift wrap + express delivery to a product

In Activity 04, the main problem is completely different:

> “The application should **not directly instantiate** these objects… provide a way to **create objects dynamically**, based on the type of file the user opens.”

This is a problem of **object creation**, not of adding extra behavior to an existing object.

---
### Key Differences

| Aspect                        | Activity 04 Requirement                  | Decorator Pattern                        |
|------------------------------|------------------------------------------|------------------------------------------|
| Main concern                 | **Creating** the correct object          | **Adding behavior** to an existing object |
| What changes                 | Which concrete class is created          | Responsibilities of an object            |
| Relationship                 | Client → Factory → Product               | Component ← Decorator (wrapping)         |
| Fits the problem?            | No                                       | No                                       |

---
### Correct Pattern for Activity 04

The correct pattern is the **Factory Pattern** (Factory Method or Simple Factory), because:
- The system needs to create different types of document objects (Word, PDF, Spreadsheet)
- Creation should be decided at runtime based on the file type
- Client code should not use `new` directly on concrete classes

---

**Summary:**  
Decorator = “Add features to an object”  
Factory = “Create the right object”  

Activity 04 is clearly about **creation**, so Factory is the right choice.