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
> **RU:** Стрелка показывает не **«кто кого содержит»**, а **по какому полю/значению можно однозначно определить значение другого поля**.

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
