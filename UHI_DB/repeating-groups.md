# Repeating Groups

**Repeating values are NOT the repeating group.**

## UNF — several orders

| OrderNo | Customer | Clerk | Item | Description | Qty | Price |
|---|---|---|---|---|---:|---:|
| **405** | **ABC** | **Martin** | 800 | small widget | 40 | 60 |
| ~~405~~ | ~~ABC~~ | ~~Martin~~ | 801 | spacer | 20 | 20 |
| ~~405~~ | ~~ABC~~ | ~~Martin~~ | 805 | large widget | 10 | 100 |
| **406** | **XYZ Ltd** | **Sarah** | 802 | bolt | 50 | 2 |
| ~~406~~ | ~~XYZ Ltd~~ | ~~Sarah~~ | 806 | washer | 50 | 1 |
| **407** | **North Co** | **James** | 803 | bracket | 12 | 15 |
| ~~407~~ | ~~North Co~~ | ~~James~~ | 804 | screw | 100 | 0.50 |
| ~~407~~ | ~~North Co~~ | ~~James~~ | 807 | plate | 4 | 25 |
| **408** | **West Ltd** | **Martin** | 800 | small widget | 5 | 60 |
| ~~408~~ | ~~West Ltd~~ | ~~Martin~~ | 805 | large widget | 2 | 100 |

The crossed-out cells are **repeating values within the same order**.

After mentally removing them:

| OrderNo | Customer | Clerk | Item | Description | Qty | Price |
|---|---|---|---|---|---:|---:|
| 405 | ABC | Martin | 800 | small widget | 40 | 60 |
|  |  |  | 801 | spacer | 20 | 20 |
|  |  |  | 805 | large widget | 10 | 100 |
| 406 | XYZ Ltd | Sarah | 802 | bolt | 50 | 2 |
|  |  |  | 806 | washer | 50 | 1 |
| 407 | North Co | James | 803 | bracket | 12 | 15 |
|  |  |  | 804 | screw | 100 | 0.50 |
|  |  |  | 807 | plate | 4 | 25 |
| 408 | West Ltd | Martin | 800 | small widget | 5 | 60 |
|  |  |  | 805 | large widget | 2 | 100 |

The **repeating group** is:

`Item + Description + Qty + Price`

It occurs several times **inside each OrderNo group**.

### How to recognise it

Look at one parent key at a time:

- `405` has 3 item rows.
- `406` has 2 item rows.
- `407` has 3 item rows.
- `408` has 2 item rows.

`OrderNo`, `Customer`, and `Clerk` describe the **order**.  
`Item`, `Description`, `Qty`, and `Price` can occur **many times for one order**.

So the repeating group is **not identified because OrderNo repeats globally**. It is identified because **one OrderNo can own several occurrences of the same child attribute set**.

> Data values alone are not always enough; the source document/business meaning confirms which attributes belong to the parent and which form the repeating child group.

## Removing Repeating Groups: UNF -> 1NF

**Keep once per order:**

`OrderNo + Customer + Clerk`

**Move the repeating child group:**

`Item + Description + Qty + Price`

### ORDER

| OrderNo | Customer | Clerk |
|---|---|---|
| 405 | ABC | Martin |
| 406 | XYZ Ltd | Sarah |
| 407 | North Co | James |
| 408 | West Ltd | Martin |

### ORDER ITEM

| OrderNo | Item | Description | Qty | Price |
|---|---|---|---:|---:|
| 405 | 800 | small widget | 40 | 60 |
| 405 | 801 | spacer | 20 | 20 |
| 405 | 805 | large widget | 10 | 100 |
| 406 | 802 | bolt | 50 | 2 |
| 406 | 806 | washer | 50 | 1 |
| 407 | 803 | bracket | 12 | 15 |
| 407 | 804 | screw | 100 | 0.50 |
| 407 | 807 | plate | 4 | 25 |
| 408 | 800 | small widget | 5 | 60 |
| 408 | 805 | large widget | 2 | 100 |

**Important:** we do **not** simply delete repeated values. Crossing them out is only a **visual technique for discovering the repeating group**.
