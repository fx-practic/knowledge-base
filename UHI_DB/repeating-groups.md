# Repeating Groups

**Repeating values are NOT the repeating group.**

## UNF

| OrderNo | Customer | Clerk | Item | Description | Qty | Price |
|---|---|---|---|---|---:|---:|
| **405** | **ABC** | **Martin** | 800 | small widget | 40 | 60 |
| ~~405~~ | ~~ABC~~ | ~~Martin~~ | 801 | spacer | 20 | 20 |
| ~~405~~ | ~~ABC~~ | ~~Martin~~ | 805 | large widget | 10 | 100 |

The crossed-out cells are **repeating values**.

After mentally removing them:

| OrderNo | Customer | Clerk | Item | Description | Qty | Price |
|---|---|---|---|---|---:|---:|
| 405 | ABC | Martin | 800 | small widget | 40 | 60 |
|  |  |  | 801 | spacer | 20 | 20 |
|  |  |  | 805 | large widget | 10 | 100 |

The **repeating group** is:

`Item + Description + Qty + Price`

It occurs several times for **one Order**.

## Removing Repeating Groups: UNF -> 1NF

**Keep once:**

`OrderNo + Customer + Clerk`

**Move the part that occurs multiple times:**

`Item + Description + Qty + Price`

### ORDER

| OrderNo | Customer | Clerk |
|---|---|---|
| 405 | ABC | Martin |

### ORDER ITEM

| OrderNo | Item | Description | Qty | Price |
|---|---|---|---:|---:|
| 405 | 800 | small widget | 40 | 60 |
| 405 | 801 | spacer | 20 | 20 |
| 405 | 805 | large widget | 10 | 100 |

**Important:** we do **not** simply delete repeated values. Crossing them out is only a **visual technique for discovering the repeating group**.
