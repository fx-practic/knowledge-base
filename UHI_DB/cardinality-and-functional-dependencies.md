# Cardinality and Functional Dependencies

## Notation Used in These Notes

When showing table structures:

- **Foreign Key**: mark the field with an asterisk `*`, for example `StudentID*`.
- **Primary Key**: underline the field, for example `<u>StudentID</u>`.
- If a field is both a **Primary Key (or part of a composite Primary Key)** and a **Foreign Key**, use both markings, for example `<u>StudentID*</u>`.

---

## Cardinality

There are two different meanings of **cardinality** that should not be confused.

### Column Cardinality

**Column cardinality** is the number of distinct values in one column.

Example:

```text
Department
----------
IT
IT
HR
HR
Sales
```

Column cardinality of `Department` = `3`.

---

### Relationship Cardinality

**Relationship cardinality** describes how values in two columns relate to each other.

- `1:1` — one to one
- `1:N` — one to many
- `N:1` — many to one
- `N:M` — many to many

Example:

```text
Employee : Manager = N:1
```

Many employees may have the same manager.

In everyday database and schema discussions, **relationship cardinality is used very often**.

---

## Functional Dependency

A functional dependency is written:

```text
A -> B
```

It means:

> **A determines B.**

For each value of `A`, there can be only one corresponding value of `B`.

### How to Read the Arrow

> **EN:** The arrow does not show **“who contains whom”**; it shows **which field/value uniquely determines the value of another field**.
>
> **RU:** Стрелка показывает не **«кто кого содержит», а **по какому полю/значению можно однозначно определить значение другого поля**.

So in:

```text
A -> B
```

`B` depends on `A`, because knowing `A` determines exactly one value of `B`.

The word **functional** comes directly from a mathematical function:

```text
y = f(x)
```

One `x` must produce exactly one `y`.

Several different `x` values may produce the same `y`.

Example:

```text
Employee -> Manager
```

Here:

```text
Employee = x
Manager  = y
```

Each employee has one manager, while one manager may manage many employees.

Therefore:

```text
Employee : Manager = N:1
Employee -> Manager
```

### Relationship Cardinality vs Functional Dependency

| Relationship cardinality A:B | Functional dependency |
|---|---|
| `1:1` | `A -> B` and `B -> A` |
| `N:1` | `A -> B` |
| `1:N` | `B -> A` |
| `N:M` | no functional dependency in either direction |

A functional dependency therefore describes **one direction of determination**, while relationship cardinality describes the relationship in both directions.

Functional dependencies are especially useful in:

- normalization
- schema design
- dependency analysis
- finding candidate keys

---

## Functional Dependencies with Multiple Columns

A functional dependency can use several columns together:

```text
(StudentID, CourseID) -> Grade
```

This means that the combination of `StudentID` and `CourseID` determines exactly one `Grade`.

The columns on the left do not have to be a key, although composite keys are a common case.

### Composite Key Components

Components of a composite key do **not** have to functionally depend on each other.

```text
(A, B) -> A   // trivial
(A, B) -> B   // trivial
A -> B        // not required
B -> A        // not required
```

If one component determines the other, the composite key may not be minimal.

---

## Common Types of Functional Dependency

| Type | Example | Meaning |
|---|---|---|
| **Full dependency** | `(StudentID, CourseID) -> Grade` | all columns on the left are required |
| **Partial dependency** | `(StudentID, CourseID) -> StudentName` | only part of the left side is actually required |
| **Transitive dependency** | `A -> B`, `B -> C` | therefore `A -> C` |
| **Trivial dependency** | `(A, B) -> A` | the right side is already included on the left |
| **Non-trivial dependency** | `A -> B` | `B` is not already included in `A` |

---

## Determinant

In:

```text
A -> B
```

`A` is called the **determinant**.

A determinant can be:

- one column
- several columns together

Example:

```text
(StudentID, CourseID) -> Grade
```

The determinant is:

```text
(StudentID, CourseID)
```

---

## Candidate Keys

A **candidate key** is a minimal column or set of columns that uniquely determines the whole row.

Example:

```text
StudentID -> StudentName, BirthDate, Email
```

If `StudentID` uniquely identifies every student and no smaller set can do so, `StudentID` is a candidate key.

A table may have several candidate keys.

One candidate key is normally selected as the **primary key**.

---

## Functional Dependencies and Normalization

Functional dependencies are the basis of normalization.

### 2NF

2NF removes **partial dependencies**.

Problem:

```text
(StudentID, CourseID) -> Grade
StudentID -> StudentName
```

`StudentName` depends only on part of the composite key.

### 3NF

3NF removes **transitive dependencies**.

Problem:

```text
EmployeeID -> DepartmentID
DepartmentID -> DepartmentName
```

Therefore:

```text
EmployeeID -> DepartmentName
```

but indirectly, through `DepartmentID`.

This is a transitive dependency.

---

## Practical Normalization Rules

### Functional dependencies are not removed

Normalization does **not** try to eliminate functional dependencies themselves. Functional dependencies are necessary.

The goal is to place attributes in tables where their dependency is appropriate.

A useful way to think about the final result is:

> **In a properly normalized table, every non-key attribute is functionally dependent on the whole Primary Key, and there are no other non-key functional dependencies inside the table.**

In practical 3NF terms, this means:

- a non-key attribute must not depend on only part of a composite Primary Key;
- a non-key attribute must not depend on another non-key attribute.

So a dependency such as:

```text
VisitID -> VisitDate
```

is good when `VisitID` is the Primary Key of `Visit`.

Normalization removes the **wrong placement of dependencies**, not dependency itself.

- **2NF** removes a dependency on only part of a composite Primary Key.
- **3NF** removes a dependency of a non-key attribute on another non-key attribute.
- After moving the attributes, the functional dependency still exists, but it exists in the correct table.

### A Foreign Key does not have to be part of the Primary Key

A table may contain a Foreign Key that is simply a non-key attribute.

Example:

| Visit |
|---|
| <u>VisitID</u> |
| VisitDate |
| ProcedureID* |

Here `ProcedureID*` is a Foreign Key, but it is **not part of the Primary Key**.

### A field can be both part of a Primary Key and a Foreign Key

Being a Foreign Key does not stop an attribute from also being part of the Primary Key.

Example:

| Treatment |
|---|
| <u>PetID*</u> |
| <u>VisitID*</u> |

Both attributes are Foreign Keys, while together they form the **composite Primary Key**.

### A table may consist only of two Foreign Keys

This is common for a **junction table** representing an M:N relationship.

Example:

| StudentUnit |
|---|
| <u>StudentID*</u> |
| <u>UnitCode*</u> |

The table may contain no other attributes. Each column is a Foreign Key, and together they form the composite Primary Key.

### Row count after normalization — course exercise observation

For the normalization exercises used in this course, when the original UNF rows contain no duplicate records that are removed, at least one resulting relation normally preserves the same number of row occurrences as the original flattened UNF data.

This is often the relationship/junction table because it preserves each original occurrence of the relationship while descriptive attributes are moved to other tables.

Treat this as a **course working rule/observation**, not as the definition of a normal form. The formal definitions of 1NF, 2NF and 3NF are based on structure and dependencies, not on a required row count.
